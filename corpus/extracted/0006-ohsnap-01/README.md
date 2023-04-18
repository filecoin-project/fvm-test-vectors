# Test vector extraction

## Database

Sentinel mainnet.

## Query

Extracting the 10 most recent messages per unique combination of (receiver_type, method_num, exit_code), starting at height 1594680.

```sql
with uniq_msgs as (
    select messages.cid                 as message_cid,
           actors.code                  as receiver_code,
           messages.method              as method_num,
           receipts.exit_code           as exit_code,
           messages.height              as height,
           block_messages.block            as block_cid,
           row_number()
           over (partition by messages.cid) as uniq_rn
    from messages -- join messages, with their blocks, their actor types, and receipts.
             join block_messages on messages.cid = block_messages.message and block_messages.height = messages.height
             join actors on messages.to = actors.id and actors.height = messages.height
             join receipts as receipts on messages.cid = receipts.message and receipts.height = messages.height + 1
    where messages.height > 1594680
    order by messages.height desc
),

group_by_type
   as -- take the previous input, and assign row numbers based on message_cid; we'll only retain unique messages.
(
   select uniq_msgs.*,
           row_number() over (partition by receiver_code, method_num, exit_code order by height desc) as group_rn
    from uniq_msgs
    where uniq_rn = 1
    order by height desc
)

select message_cid, receiver_code, method_num, exit_code, height, block_cid, group_rn as seq
from group_by_type
where group_rn <= 10
order by receiver_code, method_num, exit_code, height desc;
```
