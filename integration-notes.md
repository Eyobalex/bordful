# Airtable Integration Notes

## Configuration

The application connects to Airtable using the official `airtable` Node.js client.

### Environment Variables

| Variable                | Description                                                     | Required |
| ----------------------- | --------------------------------------------------------------- | -------- |
| `AIRTABLE_ACCESS_TOKEN` | Personal Access Token (PAT) with `data.records:read` scope.     | Yes      |
| `AIRTABLE_BASE_ID`      | The ID of the Airtable Base (starts with `app`).                | Yes      |
| `AIRTABLE_TABLE_NAME`   | The Name or ID of the Table (starts with `tbl` is recommended). | Yes      |

### API Client

- **Library**: `airtable` (npm)
- **Initialization**: `lib/db/airtable.ts`
- **Base URL**: `https://api.airtable.com` (Default)

## Integration Points

### 1. Fetch All Jobs (`getJobs`)

- **Method**: `base(TABLE_NAME).select({...}).all()`
- **Filters**: `{status} = 'active'`
- **Sorting**: By `posted_date` (descending)
- **Usage**: Fetches all active job listings for the main board and sitemap.

### 2. Fetch Single Job (`getJob`)

- **Method**: `base(TABLE_NAME).find(id)`
- **Usage**: Fetches details for a specific job page (`/jobs/[slug]`).
- **Logic**: Returns `null` if the job status is not 'active'.

## Data Schema

The application maps Airtable fields to the `Job` TypeScript interface.

| Airtable Field Name        | Data Type        | App Field                  | Notes                                     |
| -------------------------- | ---------------- | -------------------------- | ----------------------------------------- |
| `title`                    | Single Line Text | `title`                    |                                           |
| `company`                  | Single Line Text | `company`                  |                                           |
| `type`                     | Single Select    | `type`                     | Full-time, Part-time, Contract, Freelance |
| `status`                   | Single Select    | `status`                   | Must be 'active' to be visible            |
| `posted_date`              | Date             | `posted_date`              |                                           |
| `description`              | Long Text        | `description`              | Supports Markdown                         |
| `apply_url`                | URL              | `apply_url`                |                                           |
| `salary_min`               | Number           | `salary.min`               |                                           |
| `salary_max`               | Number           | `salary.max`               |                                           |
| `salary_currency`          | Single Select    | `salary.currency`          | ISO code (e.g., USD, EUR)                 |
| `salary_unit`              | Single Select    | `salary.unit`              | hour, day, week, month, year              |
| `benefits`                 | Long Text        | `benefits`                 |                                           |
| `application_requirements` | Long Text        | `application_requirements` |                                           |
| `career_level`             | Multi Select     | `career_level`             | e.g., Senior, Junior                      |
| `visa_sponsorship`         | Single Select    | `visa_sponsorship`         | Yes, No                                   |
| `workplace_type`           | Single Select    | `workplace_type`           | Remote, On-site, Hybrid                   |
| `remote_region`            | Single Select    | `remote_region`            | e.g., Worldwide, US Only                  |
| `workplace_city`           | Single Line Text | `workplace_city`           |                                           |
| `workplace_country`        | Single Line Text | `workplace_country`        |                                           |
| `languages`                | Multi Select     | `languages`                | e.g., English, French                     |
| `featured`                 | Checkbox         | `featured`                 |                                           |
| `job_identifier`           | Single Line Text | `job_identifier`           |                                           |
| `job_source_name`          | Single Line Text | `job_source_name`          |                                           |

### Structured Data Fields (Schema.org)

These fields are used for SEO rich snippets:

- `skills`
- `qualifications`
- `education_requirements`
- `experience_requirements`
- `industry`
- `occupational_category`
- `responsibilities`
