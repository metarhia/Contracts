# User account, rights and auth subsystem

Multipurpose user management subsystem. Related to [messaging](Messaging.md) and [knowledge base](KnowledgeBase.md) subsystems.


Since there are differences in information security level and access frequency we may divide following categories. 
All information should be stored in persistent data storages, such as postgres, with optional cache in redis and/or RAM. 


## Authentication
Security: highest  
Read: rare
Write: rare

- Password: String
- E-Mail: String
- Fallback E-Mail: String (optional)
- Phone: String (optional)
- Verification code: String
- Verification issued time: String
- Auth token: String
- Auth token provider: String

## Service
Security: high  
Read: intermediate
Write: rare

- Role: String
- Username: String
- Created time: Timestamp

## Info (Optional)
Security: depending on system purpose  
Read: frequent
Write: rare

- Userpicks: Json (Array?)
- Status: String
- Details: Json (Multi Level Categories)

## Counters (Optional)
Security: depending on system purpose  
Read: ultra frequent
Write: ultra frequent

- Likes: Number
- Dislikes: Number
- Last seen: Timestamp

## User relations (Optional)
Security: depending on system purpose  
Read: ultra frequent
Write: ultra frequent




