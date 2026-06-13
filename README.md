# CareOps Command Centre Demo 2

Static, data-driven CareOps Command Centre dashboard for UK / England homecare, supported living and care agencies.

The demo is for provider conversations and Readiness Sprint validation. It shows what is missing, overdue and at risk this week without replacing the care planning, rota, payroll, finance or records systems already in use.

Brightway Homecare is a fictional demo provider. All people, enquiries, referrals and shift examples are fictional. No real provider data is included.

## What is included

- `index.html` — plain static dashboard that fetches and renders `data/mock.json`.
- `data/mock.json` — fictional Brightway Homecare demo data.
- `README.md` — setup and deployment notes.
- `.gitignore` — local/editor ignores only.

There is no backend, login, build step, package install or framework.

## Run locally

Use a local HTTP server from the repository root:

```bash
python -m http.server 8765
```

Open:

```text
http://127.0.0.1:8765/
```

`file://` will not work because `index.html` uses `fetch()` to load `data/mock.json`. Browsers block that local file read for security, so the files must be served over HTTP.

## How to swap provider data

Edit `data/mock.json` only.

Keep the same top-level sections:

- `provider`
- `staff`
- `training`
- `rota_gaps`
- `referrals`
- `recruitment`
- `compliance_evidence`
- `manager_handovers`
- `base_actions`

Use fictional or anonymised data for demos. Do not put live care notes, medication details, addresses or personal records into the public demo.

## Cross-tile link rules

The dashboard derives linked warnings in JavaScript from one source record. Do not repeat the same fact manually in several modules.

### Staff DBS/file issue

If a staff member has:

- `dbs_status = expired`, or
- `staff_file_status = blocked`

then the dashboard derives:

- red/blocked Staff Readiness row;
- DBS row in Training & DBS Expiry where relevant;
- removal from usable rota cover;
- red Rota Gaps row for any assigned shift;
- Weekly Action Plan item;
- Manager Handover note.

Seed example: Daniel Okafor has an expired DBS in the staff record. That one fact appears across Staff Readiness, Training & DBS, Rota Gaps and Manager Handover.

### Unanswered enquiry/referral

If a referral/enquiry has:

- `status = new`, and
- `hours_since_received > 24`

then the dashboard derives:

- red Missed Enquiries row;
- red Referral Pipeline row;
- Weekly Action Plan item to call back today;
- Manager Handover note.

If `status = lost`, the row is grey/lost and is kept out of active action items.

Seed example: Mrs Patel has a private enquiry unanswered for 26 hours. That one referral record appears in Missed Enquiries, Referral Pipeline and the action plan.

### Training expiry or missing evidence

If training is expired or due within 30 days, the dashboard shows it in Training & DBS Expiry. If the related evidence is missing or out of date, Compliance Evidence shows an amber row. The Weekly Action Plan adds a reminder to book or update training evidence. Manager Handover adds a note when the issue is due this week.

Seed example: Sam Holt's medication training expires in 10 days and evidence is missing. That appears in Training & DBS, Compliance Evidence and the action plan.

## GitHub Pages deploy steps

1. Push the repository to GitHub.
2. Open the repository on GitHub.
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select branch `main` and folder `/ (root)`.
6. Save.
7. Open the Pages URL once GitHub finishes publishing.

## Vercel deploy steps

1. Push the repository to GitHub.
2. In Vercel, choose **Add New Project**.
3. Import `liontech82/careops-command-centre-demo2`.
4. Leave build command blank.
5. Leave output directory blank or use the repository root.
6. Deploy.

## Notes for provider demos

Use plain care-manager language:

- missing;
- overdue;
- needs attention;
- follow up;
- ready to work;
- evidence not found;
- unfilled shift;
- handover note.

Avoid technical promises or inspection outcome claims. The demo is a visibility aid for discussion, not a replacement for existing systems.
