# AI and API Validation Log

Student: JOSHUA DANREI C. RUIZ  
Section: TN35  
Date: 10/04/2026

Approved AI tool: CHATGPT

Do not paste a username, password, bearer token, API key, Authorization header,
X-API-KEY value, or private response data into this file or an AI tool.

| Request | Pre-AI prediction | Sanitized AI prompt | AI recommendation | Decision (Accepted/Modified/Rejected) | Simulator/Postman evidence |
|---|---|---|---|---|---|
| R1 | GET /api/v1/books; no authentication, parameters, or body; predicted 200 | Sanitized R1 prompt asking AI to verify the request method, endpoint, authentication, and expected status | GET /api/v1/books with no authentication or body; expected 200 | Accepted | API Simulator and Postman returned HTTP 200 with the books collection |
| R2 | GET /api/v1/books with includeISBN=true and sortBy=author; no authentication or body; predicted 200 | Sanitized R2 prompt asking AI to verify the query parameters, request design, and expected status | GET /api/v1/books with includeISBN=true and sortBy=author; expected 200 | Accepted | API Simulator and Postman returned HTTP 200 with ISBN values and books sorted by author |
| R3 | POST /api/v1/loginViaBasic using Basic Auth; no query parameters or body; predicted 200 | Sanitized R3 prompt asking AI to verify the authentication method, endpoint, and expected status without exposing credentials | POST /api/v1/loginViaBasic using Basic Auth; expected 200 | Accepted | API Simulator and Postman returned HTTP 200 OK with a temporary authentication token; token value was not recorded |
| R4 | POST /api/v1/books with a JSON body containing a fictional book; API key required; no query parameters; predicted 200 | Sanitized R4 prompt asking AI to verify the POST request design, JSON body, required API-key authentication, and expected status | POST /api/v1/books with JSON content and x-api-key authentication; expected 200 | Accepted | API Simulator returned HTTP 200 OK and created the fictional book with ID 4 |
| R5 | GET /api/v1/books/4 to retrieve the newly added book; no authentication; predicted 200 | Sanitized R5 prompt asking AI to verify the GET-by-ID endpoint, authentication requirement, and expected status | GET /api/v1/books/{id} with no authentication; expected 200 | Accepted | API Simulator and Postman returned HTTP 200 OK with the newly added book |
| R6 | DELETE /api/v1/books/4 with API-key authorization and no body; predicted 200 | Sanitized R6 prompt asking AI to verify the DELETE-by-ID endpoint, required API-key authentication, and expected status | DELETE /api/v1/books/{id} using x-api-key authentication; expected 200 | Accepted | API Simulator returned HTTP 200 OK for the deletion request |
| R7 | POST /api/v1/books with JSON body and an intentionally invalid or missing API key; predicted 401 | Sanitized R7 prompt asking AI to verify the unauthorized-request design and expected failure behavior | POST /api/v1/books without a valid API key; expected 401 | Accepted | API Simulator returned HTTP 401 Unauthorized with "Invalid API key" during the intentional failure test |

## Python script review

- Sanitized script reviewed:
  - The script uses the `requests`, `json`, and `faker` modules to automate the creation of fictional books through the School Library API.
  - The Basic Authentication credentials are defined in the script, but they were not included in this log or shared with the AI tool.
  - The API host is `http://library.demo.local`.

- Authentication workflow:
  - The `getAuthToken()` function sends a POST request to `/api/v1/loginViaBasic` using Basic Authentication.
  - If the response status is 200, the script extracts the returned token.
  - The token is then used as the `X-API-Key` value when adding books.

- Book creation workflow:
  - The `addBook()` function sends a POST request to `/api/v1/books`.
  - It sets the `Content-Type` header to `application/json` and supplies the API key through the `X-API-Key` header.
  - The book object is converted to JSON before being sent to the API.

- Faker and loop workflow:
  - The `Faker` module generates fictional book titles, author names, and ISBN-13 values.
  - The loop runs from ID 4 through ID 104, generating and submitting 101 fictional books.
  - Each generated book contains an ID, title, author, and ISBN.

- Evaluated improvement:
  - One improvement would be to add a request timeout and more controlled error handling to the HTTP requests.
  - The current script does not specify a timeout, so a request could wait indefinitely if the API becomes unresponsive.
  - Adding a timeout would make the automation safer and more reliable while still preserving the existing authentication and book-creation workflow.

- AI recommendation evaluated:
  - The AI recommendation to improve request reliability through timeout handling was accepted because it reduces the risk of the automation becoming stuck when the API does not respond.

- Independent validation performed:
  - The script structure and API workflow were compared with the documented Basic Authentication and book-creation process used in the lab.
  
## AI-use disclosure

- Assistance received:
  - CHATGPT was used to review API request designs, identify corrections, predict expected HTTP status codes, and help document validation evidence.
- Checks performed before accepting suggestions:
  - API documentation, API Simulator responses, Postman responses, and the offline request-plan validator were used to independently check the recommendations.
- Revisions made by the student:
  - Authentication labels, endpoint formats, request-plan fields, and validation evidence were revised based on the API documentation and actual simulator results.
- One limitation or error found in the AI response:
  - An earlier R3 prediction used `/api/v1/loginViaJSON`; the request structure was valid, but an earlier test returned 401 because of incorrect credentials. The request plan was subsequently aligned with the documented Basic Auth scenario and sensitive credential values were kept out of the log.