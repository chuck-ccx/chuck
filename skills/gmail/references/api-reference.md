# Gmail API Reference

Full endpoint reference for cases where `scripts/gmail.py` doesn't cover a specific operation.

## Base URL

```
https://gmail.googleapis.com/gmail/v1/users/me
```

All requests require `Authorization: Bearer <access_token>` header.

## Messages

### List Messages
```
GET /messages?maxResults=10&q=is:unread
```
Response: `{ "messages": [{"id": "...", "threadId": "..."}], "nextPageToken": "..." }`

### Get Message
```
GET /messages/{id}
GET /messages/{id}?format=metadata&metadataHeaders=From&metadataHeaders=Subject&metadataHeaders=Date
```
Formats: `full` (default), `metadata`, `minimal`, `raw`

### Send Message
```
POST /messages/send
Content-Type: application/json

{"raw": "<base64url-encoded-RFC2822-message>"}
```

### Modify Labels
```
POST /messages/{id}/modify
Content-Type: application/json

{"addLabelIds": ["STARRED"], "removeLabelIds": ["UNREAD"]}
```

### Trash / Untrash
```
POST /messages/{id}/trash
POST /messages/{id}/untrash
```

### Batch Modify
```
POST /messages/batchModify
Content-Type: application/json

{"ids": ["id1", "id2"], "addLabelIds": ["Label_1"], "removeLabelIds": ["UNREAD"]}
```

## Threads

### List Threads
```
GET /threads?maxResults=10&q=subject:meeting
```

### Get Thread
```
GET /threads/{id}
```
Returns all messages in the thread.

## Drafts

### Create Draft
```
POST /drafts
Content-Type: application/json

{"message": {"raw": "<base64url-encoded-RFC2822-message>"}}
```

### List Drafts
```
GET /drafts?maxResults=10
```

### Send Draft
```
POST /drafts/send
Content-Type: application/json

{"id": "<draft_id>"}
```

### Update Draft
```
PUT /drafts/{id}
Content-Type: application/json

{"message": {"raw": "<base64url-encoded-RFC2822-message>"}}
```

## Labels

### List Labels
```
GET /labels
```
Common system labels: `INBOX`, `SENT`, `DRAFT`, `STARRED`, `UNREAD`, `TRASH`, `SPAM`, `IMPORTANT`

### Create Label
```
POST /labels
Content-Type: application/json

{"name": "My Label", "labelListVisibility": "labelShow", "messageListVisibility": "show"}
```

## Profile
```
GET /profile
```
Returns: `emailAddress`, `messagesTotal`, `threadsTotal`, `historyId`

## Pagination

Use `nextPageToken` from response as `pageToken` query parameter to fetch next page.

## Error Codes

| Code | Meaning |
|------|---------|
| 400 | Bad request (check query syntax) |
| 401 | Token expired or invalid |
| 403 | Insufficient permissions / delegation not set up |
| 404 | Message/thread not found |
| 429 | Rate limited — back off and retry |
| 500 | Gmail server error — retry with backoff |

## Rate Limits

- 250 quota units per user per second
- List = 5 units, Get = 5 units, Send = 100 units
- Batch operations are more efficient for bulk reads
