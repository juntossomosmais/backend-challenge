<p align="center">
  <img src="https://github.com/user-attachments/assets/c16e9208-a4ce-459c-97e9-6a9f95b2f159" width="200" alt="Juntos Somos Mais">
</p>

# &lt;backend-challenge /&gt;

*[Leia em Português](./README.pt-BR.md)*

The main objective of this challenge is to assess your approach to **problem-solving, code quality, and how you leverage modern tools** — including AI.

We evaluate:

- Your coding style and organization
- Decision-making and trade-offs
- Testing strategies
- Documentation quality
- How you use AI as a development tool

> 🤖 **AI is welcome here.** We don't want to know *if* you used AI. We want to know *how* you used it.

---

## Table of Contents

- [The Challenge](#the-challenge)
- [Business Rules](#business-rules)
- [API Requirements](#api-requirements)
- [Evaluation Criteria](#evaluation-criteria)
- [AI Journey (Required)](#ai-journey-required)
- [Submission](#submission)
- [FAQ](#faq)

---

## The Challenge

We receive customer data from partner companies in both **CSV** and **JSON** formats. Your task is to:

1. **Load** data from external URLs at application startup
2. **Transform** the data applying our business rules
3. **Expose** a REST API to query the processed data

### Input Data

| Format | URL | Records |
|--------|-----|---------|
| CSV | [input-backend.csv](https://storage.googleapis.com/juntossomosmais-code-challenge/input-backend.csv) | ~1000 |
| JSON | [input-backend.json](https://storage.googleapis.com/juntossomosmais-code-challenge/input-backend.json) | ~1000 |

> ⚠️ Data must be loaded via HTTP request **at startup** and kept **in memory**. No database required.

---

## Business Rules

### 1. Customer Classification by Location

Based on coordinates, classify each customer:

| Type | Bounding Box |
|------|--------------|
| **SPECIAL** | minlon: -2.196998, minlat: -46.361899, maxlon: -15.411580, maxlat: -34.276938 |
| **SPECIAL** | minlon: -19.766959, minlat: -52.997614, maxlon: -23.966413, maxlat: -44.428305 |
| **NORMAL** | minlon: -26.155681, minlat: -54.777426, maxlon: -34.016466, maxlat: -46.603598 |
| **LABORIOUS** | Anyone not matching the above |

### 2. Data Transformations

| Field | Transformation |
|-------|----------------|
| `phone`, `cell` | Convert to [E.164](https://en.wikipedia.org/wiki/E.164) format. Example: `(86) 8370-9831` → `+558683709831` |
| `gender` | `male` → `M`, `female` → `F` |
| `dob.age`, `registered.age` | Remove these fields |
| `nationality` | Add field with value `BR` |
| `region` | Add based on state (Norte, Nordeste, Centro-Oeste, Sudeste, Sul) |

### 3. Output Contract

```json
{
  "type": "laborious",
  "gender": "M",
  "name": {
    "title": "mr",
    "first": "quirilo",
    "last": "nascimento"
  },
  "location": {
    "region": "sul",
    "street": "680 rua treze",
    "city": "varginha",
    "state": "paraná",
    "postcode": 37260,
    "coordinates": {
      "latitude": "-46.9519",
      "longitude": "-57.4496"
    },
    "timezone": {
      "offset": "+8:00",
      "description": "Beijing, Perth, Singapore, Hong Kong"
    }
  },
  "email": "quirilo.nascimento@example.com",
  "birthday": "1979-01-22T03:35:31Z",
  "registered": "2005-07-01T13:52:48Z",
  "telephoneNumbers": ["+556629637520"],
  "mobileNumbers": ["+553270684089"],
  "picture": {
    "large": "https://randomuser.me/api/portraits/men/83.jpg",
    "medium": "https://randomuser.me/api/portraits/med/men/83.jpg",
    "thumbnail": "https://randomuser.me/api/portraits/thumb/men/83.jpg"
  },
  "nationality": "BR"
}
```

---

## API Requirements

### Endpoint

```
GET /users
```

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `region` | string | Filter by region (norte, nordeste, centro-oeste, sudeste, sul) |
| `type` | string | Filter by classification (special, normal, laborious) |
| `pageNumber` | int | Page number (1-indexed) |
| `pageSize` | int | Items per page |

### Response

```json
{
  "pageNumber": 1,
  "pageSize": 10,
  "totalCount": 2000,
  "users": [...]
}
```

### Validation

Your API must pass our validation script:

```bash
./validate.sh
```

This checks:
- Endpoint responding at `localhost:8080`
- Pagination fields present
- Total count of 2000 records

---

## Evaluation Criteria

We assess your submission across **7 competencies**. There are no "levels" to choose — just deliver your best work, and we'll evaluate where you stand.

### 1. 🎯 Problem Solving

| What we look for |
|------------------|
| Correct implementation of all business rules |
| Edge cases handling (invalid data, missing fields, malformed input) |
| Logical and efficient approach to data transformation |

### 2. 🏗️ Code Architecture

| What we look for |
|------------------|
| Clear separation of concerns |
| Consistent project structure |
| Appropriate use of design patterns (when they add value, not for show) |
| Code that's easy to navigate and understand |

### 3. ✨ Code Quality

| What we look for |
|------------------|
| Readability over cleverness |
| Meaningful naming conventions |
| Consistent style throughout |
| No unnecessary complexity |
| Proper error handling |

### 4. 🧪 Testing

| What we look for |
|------------------|
| Tests that document behavior |
| Coverage of critical paths |
| Tests that would catch real bugs |
| Balance between unit and integration tests |

### 5. 📚 Documentation

| What we look for |
|------------------|
| Clear README with setup instructions |
| API documentation (any format) |
| Comments where code isn't self-explanatory |
| Architecture decisions explained (when relevant) |

### 6. 🚀 Production Readiness

| What we look for |
|------------------|
| Containerization (Docker) |
| Environment configuration |
| Health checks |
| Logging strategy |
| CI/CD awareness |

### 7. 🤖 AI Collaboration

| What we look for |
|------------------|
| Transparency in AI usage |
| Critical thinking about AI-generated code |
| Iteration and refinement over copy-paste |
| Understanding of what the AI produced |

---

## AI Journey (Required)

Create an `/ai-journey` folder in your repository documenting how you used AI tools.

### Required Files

```
📁 ai-journey/
├── README.md          # Summary of your AI usage
├── prompts.md         # Key prompts you used
└── learnings.md       # What you learned in the process
```

### What to Document

**prompts.md** — Don't document everything, just the interesting parts:

```markdown
## Prompt: Phone number regex
**Tool:** ChatGPT-4

**What I asked:**
"Create a regex to convert Brazilian phone numbers to E.164 format"

**What happened:**
Initial regex didn't handle 9-digit mobile numbers. I had to...

**Final solution:**
[your code]
```

**learnings.md** — Reflect on the experience:

```markdown
## What worked well
- AI was great for boilerplate code
- Helped me explore libraries I wasn't familiar with

## What didn't work
- Initial architecture suggestion was over-engineered
- Had to simplify after understanding the actual requirements

## What I'd do differently
- Start with clearer requirements in prompts
- Ask for simpler solutions first
```

---

## Submission

### Languages

**Python** or **C#** — choose the one you're most comfortable with.

### Repository Structure

```
📁 your-repo/
├── src/                  # Source code
├── tests/                # Tests
├── ai-journey/           # AI documentation (required)
├── docker-compose.yml    # If applicable
└── README.md             # Setup instructions
```

### How to Submit

1. Create a **public** GitHub repository
2. Open an **Issue** in this repository with:
   - Title: `[Backend] Your Name`
   - Link to your repository
   - Brief description of your approach
   - Anything you'd like us to know

### Timeline

- **Recommended:** 7 days
- **Need more time?** Just let us know in the issue

---

## FAQ

<details>
<summary><b>What languages can I use?</b></summary>

Python or C#. Choose the one you're most comfortable with.
</details>

<details>
<summary><b>Are there open positions?</b></summary>

Not always, but we maintain a talent pool. Great submissions stay on our radar for future opportunities.
</details>

<details>
<summary><b>What if I can only complete part of the challenge?</b></summary>

Submit what you have! Partial submissions with quality code tell us more than complete submissions with poor code. Just document what's missing and why.
</details>

<details>
<summary><b>Should I include extra features?</b></summary>

Only if they add clear value and don't compromise the core requirements. We prefer well-executed basics over half-finished extras.
</details>

<details>
<summary><b>How will I know my seniority level?</b></summary>

We don't ask you to self-declare a level. We evaluate your submission across all criteria and determine fit based on our internal standards.
</details>

---

## Other Challenges

If you're applying for a front-end position, check out our [frontend-challenge](https://github.com/juntossomosmais/frontend-challenge).

---

## Questions?

Open an [issue](../../issues) or reach out to **vagas-dev@juntossomosmais.com.br**.

Before asking, please check if your question was already answered in [previous issues](../../issues?q=is%3Aissue).

---

<p align="center">
  <sub>Made with 💛 by the Engineering Team at <a href="https://juntossomosmais.com.br">Juntos Somos Mais</a></sub>
</p>
