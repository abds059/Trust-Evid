# Trust-Evid

**Digital Evidence & Case Management Platform**

Trust-Evid is a digital evidence and case management platform designed to help teams organize submitted evidence, review technical findings, and maintain a recorded history of evidence-related activity.

It combines secure case access, private evidence storage, deterministic media processing, optional AI-assisted triage, and case reporting in one workflow.

> **Important:** Trust-Evid supports evidence organization and review. Its hashes, metadata, processing results, AI analysis, and custody records do not independently establish authenticity, intent, or legal admissibility.

---

## Why Trust-Evid?

Digital evidence can be difficult to organize and review across files, notes, technical observations, and case activity.

Trust-Evid brings these elements together so authorized users can:

- Organize evidence within cases.
- Preserve access to submitted original files.
- Review file hashes and available metadata.
- Inspect deterministic processing findings.
- View optional AI-assisted triage as advisory analysis.
- Follow recorded custody activity and review notes.
- Export structured case reports.

## Features

### Case & Access Management

- Case-oriented evidence workspace.
- Supabase authentication with email/password login.
- TOTP multi-factor authentication (AAL2).
- Role-aware access to protected case and evidence operations.

### Evidence Intake & Preservation

- Private evidence storage.
- Signed upload URLs for evidence intake.
- SHA-256 calculation over stored original bytes.
- Write-once hash persistence in the intended finalization workflow.
- Configured upload and processing limits.

### Supported Evidence Formats

The application documents support for:

- JPEG
- PNG
- MP4

### Technical Processing

The processing worker can produce:

- **SHA-256 hashes** — file identification through content hashing.
- **Image metadata** — available metadata and EXIF details.
- **Video metadata** — metadata extracted using `ffprobe`.
- **Frame-sample records** — deterministic records from video frame sampling.

Technical findings are recorded separately from AI analysis.

### AI-Assisted Triage

- Optional Gemini-powered analysis of deterministic findings.
- Structured, bounded, schema-validated advisory output.
- AI observations may include:
  - Summary
  - Anomalies
  - Uncertainty and limitations
  - Suggested next steps
  - Review priority
- Incomplete or unavailable AI results do not replace deterministic findings.

### Custody, Review & Reporting

- Recorded custody event history.
- Review states and priorities.
- Examiner notes.
- JSON evidence report downloads.
- Reports bringing together case details, hashes, findings, review information, custody history, and stated limitations.

---

## How It Works

1. **Create or access a case**  
   Authorized users enter the appropriate case workspace.

2. **Submit evidence**  
   Evidence metadata is registered, and the original file is uploaded to private storage.

3. **Process the stored file**  
   The backend dispatches the evidence identifier to the processing worker. The worker retrieves the original through server-side access.

4. **Review technical results**  
   Examine the file hash, available metadata, and deterministic processing findings.

5. **Review optional AI triage**  
   When enabled and available, AI-generated observations are presented as advisory information.

6. **Review activity and generate a report**  
   Authorized users inspect recorded custody activity, review notes, and the structured case report.

---

## Technology Stack

| Component | Technology |
|---|---|
| Web application | Next.js App Router |
| Authentication | Supabase Auth |
| Database | Supabase |
| Evidence storage | Supabase Storage |
| Processing service | FastAPI, Python |
| Hashing | SHA-256 |
| Video processing | `ffprobe` |
| Optional AI analysis | Gemini |
| Application testing | Vitest |
| Worker testing | pytest |

---

## Project Structure

The repository contains the web application and a `worker/` service for evidence processing.

The worker includes components for:

- Application entry point
- Configuration and processing limits
- Database access
- Evidence processing
- Hashing
- AI analysis

Refer to the repository for the current directory layout and implementation details.

---

## Getting Started

### Prerequisites

- Node.js and npm compatible with the project configuration.
- Python and pip for worker development and testing.
- A configured Supabase project for authentication, database, and private Storage.
- Worker configuration for evidence-processing features.

Check the repository's package manifests for the exact runtime versions and available scripts.

### 1. Run the Web Application

From the application root:

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

### 2. Run Worker Tests

From the worker directory:

```bash
cd worker
pip install -r requirements.txt
pytest
```

### 3. Run Application Checks

If the corresponding scripts are defined in the current `package.json`:

```bash
npm test
npm run typecheck
npm run build
```

---

## Configuration

Configure the required environment variables through your local environment or deployment platform.

| Variable | Purpose |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase client key |
| `SUPABASE_SERVICE_ROLE_KEY` | Privileged server/worker access |
| `WORKER_URL` | Processing worker base URL |
| `WORKER_SHARED_SECRET` | Authenticates application requests to the worker |
| `GEMINI_API_KEY` | Optional Gemini analysis |
| `AI_MODEL` | Optional AI model selection |
| `AI_FORCE_FAIL` | Optional failure-testing switch, if supported |

The web application and worker require the variables relevant to the features being run.

> **Security:** Never commit secrets or expose privileged service keys and shared secrets to browsers or public repositories.

---

## Security & Privacy

Trust-Evid uses authentication, multi-factor authentication, case-role checks, private storage, and server-side processing to protect evidence workflows.

- Protected operations require the applicable authentication assurance and case permissions.
- The worker receives an evidence identifier rather than the user's access token.
- Private originals are retrieved by the worker using server-side credentials.
- In the documented integration, AI analysis receives deterministic findings; GPS and personal EXIF are excluded.
- Reports and findings are available only through protected application workflows.

**Testing and sensitive data:** Use synthetic or appropriately authorized material when testing. Evidence files may contain sensitive or personally identifying information.

---

## Understanding the Results

Trust-Evid is intended to assist with evidence organization and review, not to make definitive forensic or legal determinations.

- **SHA-256 hashes:** Help identify whether two byte sequences match. They do not establish who created a file, when it was created, or whether its contents are truthful.
- **Metadata:** May be missing, altered, inaccurate, or removed during file handling.
- **Deterministic findings:** Describe the output of defined technical checks. They are not proof of tampering.
- **AI analysis:** Is advisory and may be incomplete or incorrect.
- **Custody history:** Reflects recorded events and the protections implemented by the deployed system. It does not independently prove authenticity or legal admissibility.

---

## License

MIT License
