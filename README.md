# Provynk — The Global Expertise & Talent Network

**Live site:** [www.provynk.com](https://www.provynk.com)

> Enterprise tech talent, matched with the teams that need it.

Provynk is a global network for **SAP and enterprise technology professionals**, the **employers** who hire them, and the **consultancies** who evaluate them. It brings experienced professionals, niche specialists, consulting partners and organisations onto one collaborative platform, so the right expertise reaches the right opportunity.

Built on a strong SAP and enterprise-transformation foundation, the network extends across business applications, cloud, data & analytics, AI, cybersecurity, infrastructure and emerging technologies.

---

## Table of Contents

- [Features](#features)
- [Account Types](#account-types)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Firebase Setup](#firebase-setup)
- [Deployment](#deployment)
- [Routes](#routes)
- [Security Notes](#security-notes)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## Features

### For Candidates / Consultants
- Build a rich profile: headline, roles, hard and soft skills, experience, education, certifications, availability
- **Resume upload with ATS score**: CV is parsed (PDF) and scored, with skills auto-detected
- Profile-strength meter and tips to rank higher in employer searches
- Search jobs by role, hard skill, soft skill, location, job type and industry
- One-click apply and application tracking (Applied → Screened → Interview → Offer / Rejected)
- Control visibility to employers

### For Employers
- Post, edit and manage job openings
- Hiring funnel and applicant pipeline dashboard
- Search the talent pool by skill, role, industry, experience and ATS score
- Shortlist candidates and manage company details

### For Consultancies / Groups
- Create groups and invite colleagues by email
- Search individuals and companies as a team
- Shared shortlist with notes
- **Team analysis**: scans member CVs and produces a member sheet, skill matrix, seniority and education breakdown, and a required-skills check (export-ready via spreadsheet support)

### Platform
- Email/password and email-link authentication with email verification and password reset
- Role-aware navigation and dashboards
- Services, Hire, Our Team, Contact and SAP Blog sections
- Live **Newsroom** (AI, GCC in India, IT news)
- Responsive, single-page experience with hash-based routing

---

## Account Types

| Account | Who it's for | What they do |
|---|---|---|
| **Candidate** | SAP / enterprise tech professionals | Showcase skills, get an ATS score, find and apply to jobs |
| **Employer** | Companies, consultancies, staffing agencies | Post roles, review applicants, search talent |
| **Group** | Consultancies and firms | Evaluate talent together with a shared shortlist and team analytics |

---

## Tech Stack

- **Frontend:** Vanilla HTML, CSS and JavaScript (ES modules) in a single-page app, no build step
- **Backend as a service:** [Firebase](https://firebase.google.com/) Authentication and Cloud Firestore (v10.12.2)
- **Libraries (via CDN):**
  - [PDF.js](https://mozilla.github.io/pdf.js/) 3.11.174, resume parsing
  - [Chart.js](https://www.chartjs.org/) 4.4.1, dashboards and analytics
  - [SheetJS (xlsx)](https://sheetjs.com/) 0.18.5, spreadsheet import/export
- **Fonts:** Plus Jakarta Sans, Sora, Newsreader, DM Mono (Google Fonts)
- **Optional integrations:** EmailJS for group invitation emails; external ATS analysis API with a built-in local fallback scorer

---

## Project Structure

```
.
├── index.html      # Entire application (markup, styles, and scripts)
└── README.md
```

The whole app lives in `index.html`. Routing is hash-based (`#/jobs`, `#/profile`, and so on), so it works on any static host.

---

## Getting Started

### Prerequisites
- A modern web browser
- A free [Firebase](https://console.firebase.google.com/) project
- Any static file server (for local development)

### Run locally

```bash
# Clone the repository
git clone https://github.com/<your-username>/provynk.git
cd provynk

# Serve with any static server, e.g.
npx serve .
# or
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

> Firebase Auth and ES modules need `http://` or `https://`; opening the file directly via `file://` will not work.

---

## Configuration

Inside `index.html`, find the `CONFIG` object near the top of the module script and replace it with your own Firebase web config:

```js
const CONFIG = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
  measurementId: "YOUR_MEASUREMENT_ID"
};
```

**Optional settings**

| Setting | Purpose |
|---|---|
| `ATS_API` | Endpoint for server-side ATS resume analysis. If unavailable, the app falls back to local scoring. |
| `EMAILJS` | `{ service, template, key }` to send group invitation emails. If left empty, invitations appear in the invitee's **My groups** page instead. |

---

## Firebase Setup

1. Create a project in the [Firebase Console](https://console.firebase.google.com/).
2. Add a **Web app** and copy its config into `CONFIG`.
3. Enable **Authentication → Sign-in method**:
   - Email/Password
   - Email link (passwordless)
4. Add your domain under **Authentication → Settings → Authorized domains**.
5. Create a **Cloud Firestore** database.
6. Publish Firestore security rules that protect user data (see [Security Notes](#security-notes)). The app uses collections for users/profiles, jobs, applications, groups, invitations and resumes.

---

## Deployment

Because it is a static site, Provynk can be hosted anywhere:

- **GitHub Pages:** Settings → Pages → deploy from the `main` branch
- **Firebase Hosting:** `firebase init hosting && firebase deploy`
- **Netlify / Vercel / Cloudflare Pages:** drag-and-drop or connect the repo

For a custom domain such as `www.provynk.com`, point your DNS records to your host and add the domain to Firebase **Authorized domains**.

---

## Routes

| Route | Description |
|---|---|
| `#/` | Landing page |
| `#/jobs` | Search and browse jobs |
| `#/profile` | Candidate profile and ATS score |
| `#/applications` | Candidate application tracker |
| `#/mygroups` | Group invitations and memberships |
| `#/talent` | Talent and company search |
| `#/dashboard` | Employer dashboard (`?tab=post`, `?tab=jobs`, `?tab=apps`, `?tab=co`) |
| `#/groups` | Consultancy group management |
| `#/analysis` | Team skill analysis |
| `#/services` · `#/team` · `#/contact` · `#/blog` | Marketing and content pages |

---

## Security Notes

- Firebase web API keys identify your project and are safe to ship in client code, **but only if your Firestore security rules are strict**. Always restrict reads and writes by `request.auth.uid` and account type.
- Restrict the API key by HTTP referrer in the Google Cloud Console.
- Never commit service-account keys, admin credentials, or EmailJS private keys.
- Candidate CVs contain personal data. Apply appropriate consent, retention and privacy practices (GDPR / DPDP as applicable).

To report a vulnerability, please email the contact address below rather than opening a public issue.

---

## Contributing

Contributions are welcome.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## License

Copyright © Provynk. All rights reserved.

If you intend to open-source the project, replace this section with your chosen license (for example MIT) and add a `LICENSE` file.

---

## Contact

- **Website:** [www.provynk.com](https://www.provynk.com)
- **Issues:** use the GitHub Issues tab for bugs and feature requests
