## Log submission Scenarios

#### Instructions

- Create a fork of the [verification-logs](https://github.com/ONDC-Official/verification-logs) repository.
- Create a folder with the name of your entity under your domain folder "TRV14"
- Commit your logs in the folder (logs should include request & response payloads for all enabled APIs as per the scenarios below)
- Create PR and label it with your domain name
- Once submitted, please refer to the comments on logs submitted and update the PR based on the comments provided
- Once the reviews are done, the PR will be merged and the logs shall be considered as approved on PR merge
- Both IGM and transactional logs need to be submitted in a single PR, while RSF logs should be submitted in a separate PR
- For IGM logs, create a folder with name igm under your entity named folder.
- For RSF logs, create a folder with name RSF under your entity named folder.

### File Naming conventions:

1. **Single Endpoint Naming**:

   - For a single API endpoint, the file name should precisely match the name of the endpoint. For example:
     - `select: select.json` (for the "select" endpoint)
     - `on_select: on_select.json` (for the "on_select" endpoint)
2. **Multiple Calls for Same Endpoint**:

   - When there are multiple API calls for the same endpoint, the naming convention should reflect the sequence of calls using numeric suffixes:
     - `search 1: search_1.json` (for the first call of the "search" endpoint)
     - `search 2: search_2.json` (for the second call of the "search" endpoint)

These naming conventions ensure clear identification and organization of files based on the corresponding API endpoints and their respective calls.

## Scenarios

1. Pagination flow
   - On_search to be sent in parts in case of a bigger catalogue
2. Purchase of 1 ticket with add on
   - Purchase of ticket-> selection of type of ticket(premium/economy)-> Child/adult-> location-> fulfilment(visit)-> payment completion
3. Purchase of 2 tickets under 1 parent with 2 different variants (Optional)
   - Eg. 1 adult ticket under home and heritage along with 1 child ticket under parent home and heritage
4. Technical cancellation flow
   - The provider has confirmed the request, but the buyer app received the request after the TTL had expired.
   - Buyer app request for latest order status
   - Provider sends the latest order status
   - Buyer app requests for cancellation of ticket.
   - Provider accepts the terms of order and appends its own terms and provides with latest order update.
   - Buyer app give the confirmation to cancel the ticket.
   - Provider accepts the terms of order and provides with latest order update.
5. User cancellation flow
   - Buyer app requests for cancellation of ticket
   - Provider accepts the terms of order and appends its own terms and provides with latest order update(soft cancel)
   - Buyer app give the confirmation to cancel the ticket.
   - Provider accepts the terms of order and provides with latest order update.
6. Partial cancellation flow
   - Buyer app sends the request for partial cancel with soft cancel request
   - Provider app return the updated order with soft_cancel status.
   - Buyer app sends the request for partial cancel with confirm cancel request
   - Provider app return the order with updated status
7. Cancellation rejected flow
   - Buyer app requests for cancellation of ticket.
   - The provider app rejected the cancellation as it is not allowed.
8. IGM Flow 2.0
   - Issue redressal flow 2.0
9. RSF Flow 2.0
   - Reckon and settletement flow 2.0

#### For Igm logs, use POST api exposed at [https://log-validation.ondc.org/api/validate/igm](https://log-validation.ondc.org/api/validate/igm)

The body structure for igm logs:

<!-- ```json
{
  "domain": "",
  "version": "1.0.0",
  "payload": {
    "ret_issue": {},
    "ret_issue_close": {},
    "ret_on_issue": {},
    "ret_issue_status": {},
    "ret_on_issue_status": {},
    "ret_on_issue_status_unsolicited": {}
  }
} -->

```json
  {
    "domain": "<use-case domain>",
    "version": "2.0.0",
    "bap_id": "BUYER_APP_SUBSCRIBER_ID",
    "flow": "FLOW_1",
    "bpp_id": "SELLER_APP_SUBSCRIBER_ID",
    "payload": {
        "issue_open": {},
        "on_issue_processing_1":{},
        "on_issue_info_required": {},
        "issue_info_provided": {},
        "on_issue_processing_2": {},
        "on_issue_resolution_proposed": {},
        "issue_resolution_accepted": {},
        "on_issue_resolved": {},
        "issue_closed": {}
    }
```

#### For RSF logs, use POST api exposed at [https://log-validation.ondc.org/api/validate/rsf](https://log-validation.ondc.org/api/validate/rsf)

```json
    {
    "domain": "<use-case domain>",
    "version": "2.0.0",
    "bap_id": "BUYER_APP_SUBSCRIBER_ID",
    "bpp_id": "SELLER_APP_SUBSCRIBER_ID",
    "payload": {
        "settle":{},
        "on_settle":{},
        "report":{},
        "on_report":{},
        "recon":{},
        "on_recon":{}
      }
    }'
```

The api call sequence inside the payload object might differ based on different flows

_Note: Log verification will follow a FIFO model with a TAT of 4 days_
