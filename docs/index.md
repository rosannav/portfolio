# Rosanna van Hespen
*Technical Writer & Strategic Communicator*  
I turn chaotic expert knowledge into clear, actionable documents and decisions.

## Portfolio
| Piece                                                                       | Type                        |
| --------------------------------------------------------------------------- | --------------------------- |
| [Open Library Book Search API](#Open-Library-Book-Search-API-Documentation) | API reference documentation |
| [AO3 Quick-Start Guide](#AO3-Quick-Start-Guide)                             | End-user onboarding guide   |

### Open Library Book Search API Documentation
**Scope:** Search endpoint 

**About Open Library**  
The Open Library is an online library and archive. It allows users to search and lend books, and links to other known book catalogues (e.g. the Internet Archive, Kobo, Goodreads).

**The problem**
The existing documentation was limited and fragmented.

**Approach**  
I documented the call and response parameters for the Book Search endpoint by testing the endpoint directly, inferring parameter behavior from the live application, and cross-referencing original sources.

- Created a structured reference document for call and response parameters.
- Documented the `fields` parameter behavior, including which fields are available and how they filter the response.
- Organized the `sort` options into a structured table; the original documentation only referenced a git commit.
- Recorded and tested response parameters, identifying which are returned and how the JSON object is structured.
- Inferred parameter descriptions by studying the Open Library website and comparing against live API responses.
- Referenced and archived original documentation to ensure accuracy and traceability.

**Outcome**
A structured API reference document with clear descriptions of call and response parameters and special behaviour. 

### AO3 Quick-Start Guide
**Scope:** New users wanting to find a story to read

**About the platform**
Archive of Our Own is a large fan-fiction archive that hosts almost 20 million user-generated works that can be filtered and sorted based on a user-generated tagging system.

**The problem**
The existing FAQ was thorough, but dense and scattered, making it difficult for new readers to figure out how the site works and find a story to read. 

**Approach**  
I approached this as a UX writing problem: mapping the new-user journey, then applying progressive disclosure to reduce cognitive load at each step.

- Walked through the new-user journey to identify friction points and unclear terminology.
- Applied progressive disclosure principles to create a simple entry pathway.
- Balanced completeness against scope: identified what information new users need immediately versus what they can learn later
- Handled a sensitive classification system clearly and neutrally.

**Outcome**
A simple, user-oriented quick-start guide that helps users to find a story. 