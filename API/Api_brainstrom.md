## CS355 API Selection Guide

> The grades based on the **hardest authentication model** you use.

### Authentication Score Ceiling Table

| Authentication Type          | Max Score |
| ---------------------------- | --------- |
| Public API only              | 0%        |
| API Key                      | 90–95%    |
| OAuth 2.0 Client Credential  | 100%      |
| OAuth 2.0 Authorization Code | 105–110%  |


#### BAD OPTION

```text id="u9vqlp"
Public API + Public API
```

Result:

* automatic failure

#### SAFE OPTION

```text id="96mjlwm"
OAuth Client Credential API
+
Public/API Key API
```

Result:

* eligible for 100%

---

### What APIs MUST Support

Your APIs MUST:

| Requirement             | Why                            |
| ----------------------- | ------------------------------ |
| REST API                | So you can use https.request() |
| HTTPS endpoints         | Required for Node HTTPS module |
| JSON responses          | Easier parsing                 |
| No SDK dependency       | npm libraries banned           |
| cURL examples preferred | Easier testing in Bruno        |

---

### APIs You CANNOT Use

Your professor already used these:

| API             | Status |
| --------------- | ------ |
| Dictionary API  | BANNED |
| USAJobs         | BANNED |
| GitHub REST API | BANNED |
| Spotify API     | BANNED |

* Your APIs MUST Work Without

| Forbidden          | Why    |
| ------------------ | ------ |
| npm install        | banned |
| SDK/helper library | banned |
| axios              | banned |
| fetch              | banned |
| async/await        | banned |
| Promise            | banned |
| Express            | banned |

---

## API Requirements Checklist - BEFORE Choosing APIs

### Authentication

* [ ] At least ONE API uses OAuth Client Credential
* [ ] Can retrieve access token manually
* [ ] Token can be used in Authorization header


### REST Compatibility

* [ ] API works with normal HTTPS requests
* [ ] API has URL endpoints
* [ ] API returns JSON
* [ ] API can be tested in Bruno/Postman

### CS355 Compatibility

* [ ] No SDK required
* [ ] No npm package required
* [ ] Can use only https.request()
* [ ] Documentation shows REST examples

### Synchronous Compatibility

* [ ] API #2 can logically depend on API #1
* [ ] API #2 waits for API #1 response
* [ ] No user interaction between APIs

> [!IMPORTANT]
> API #2 MUST NOT happen until API #1 fully finishes.

---

### OPTION 1 — RAWG + Twitch

* (BEST OVERALL)

| API            | Auth Type               |
| -------------- | ----------------------- |
| RAWG Games API | API Key                 |
| Twitch API     | OAuth Client Credential |

---

### Why It’s Excellent

| Requirement                   | Covered |
| ----------------------------- | ------- |
| OAuth Client Credential       | Twitch  |
| Strong synchronous dependency | Yes     |
| Good visuals                  | Yes     |
| Easy HTML                     | Yes     |
| Easy screencast               | Yes     |
| Easy sequence diagram         | Yes     |

---

### Flow

```text id="icvkup"
User searches game
    ↓
RAWG returns game info
    ↓
Extract game title
    ↓
Twitch searches streams
    ↓
Display streams + ratings
```
---

### APIs You Should AVOID

| API Type            | Why Avoid               |
| ------------------- | ----------------------- |
| OAuth 1.0           | Not covered in class    |
| GraphQL-only APIs   | Harder with raw HTTPS   |
| APIs requiring SDK  | Violates rules          |
| APIs with poor docs | Hard debugging          |
| Browser-only APIs   | Difficult backend usage |

---

### What To Test BEFORE Locking APIs

## In Bruno/Postman

For EACH API:

* [ ] Can authenticate
* [ ] Can send request
* [ ] Can receive JSON
* [ ] Understand headers
* [ ] Understand query params
* [ ] Understand rate limits
* [ ] Understand response structure

---

### Screenshots You Need

Your professor specifically wants:

| Screenshot             | Required |
| ---------------------- | -------- |
| API #1 working         | Yes      |
| API #2 working         | Yes      |
| Authentication success | Yes      |

---

## Final Phase 1 Checklist

### API Selection

* [ ] Choose two APIs
* [ ] One API uses OAuth Client Credential
* [ ] APIs are NOT banned demos
* [ ] APIs support REST

### Authentication

* [ ] Create developer accounts
* [ ] Get API keys
* [ ] Get Client ID / Secret
* [ ] Successfully retrieve access token

### Testing

* [ ] Test APIs in Bruno
* [ ] Save screenshots
* [ ] Verify JSON responses
* [ ] Verify headers

### Architecture

* [ ] Draft sequence diagram
* [ ] Confirm synchronous dependency
* [ ] Plan callback chain

### CS355 Restrictions

* [ ] No npm
* [ ] No Express
* [ ] No async/await
* [ ] No Promise
* [ ] No fetch()
* [ ] Only Node.js core modules
