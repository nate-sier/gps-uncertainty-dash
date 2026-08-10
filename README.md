# Practice Plan Uncertainty Dashboard

GitHub / Streamlit Community Cloud version of the practice-plan compliance, CMJ flagging, workload, decision-audit, and recommendation dashboard.

## Repository files

- `streamlit_app.py` — app entrypoint
- `requirements.txt` — Python dependencies for Streamlit Community Cloud
- `.streamlit/secrets.toml.example` — template for Google Sheets credentials
- `.gitignore` — prevents credentials and generated CSV files from being committed

## 1. Put this folder in GitHub

Create a GitHub repository (private is recommended for an internal baseball dashboard) and commit the files in this folder.

Do **not** commit your service-account JSON or `.streamlit/secrets.toml`.

## 2. Configure Google Sheets access

The app expects two private Google Sheets:

- Python Reports workbook containing `Jump Data`, `Master Roster`, and `PP_Sprint`
- STATSports workbook containing `Raw Sessions`

Share both workbooks with the `client_email` from your Google service-account key. Use **Editor** access if you want the app to write Player Date, Decision Table, summaries, and dose-response tables back to Google Sheets.

## 3. Configure Streamlit Secrets locally

Copy the example file:

```bash
cp .streamlit/secrets.toml.example .streamlit/secrets.toml
```

Then paste your two Google Sheet URLs and the fields from your service-account JSON into `.streamlit/secrets.toml`.

Run locally:

```bash
python3 -m pip install -r requirements.txt
python3 -m streamlit run streamlit_app.py
```

## 4. Deploy on Streamlit Community Cloud

1. Push the repository to GitHub.
2. In Streamlit Community Cloud, create a new app from the repository.
3. Set the entrypoint to `streamlit_app.py`.
4. Open the app's **Secrets** settings.
5. Paste the same contents you use in `.streamlit/secrets.toml`.
6. Deploy the app.

The Streamlit Cloud machine does not need, and will not have access to, `/Users/nsier/Desktop/service_account.json`. Credentials are read from `st.secrets` in production.

## Data refresh behavior

The app does not automatically rebuild the Google Sheets data on every Streamlit rerun. Use **Build / refresh from Google Sheets** in the sidebar. The rebuilt player-date panel is stored in Streamlit session state for the current app session.

## Security

- Keep the GitHub repository private if the dashboard or code structure is internal.
- Never upload the service-account JSON to GitHub.
- Never commit `.streamlit/secrets.toml`.
- If a credential is ever committed to GitHub, revoke that Google key and generate a new one.
