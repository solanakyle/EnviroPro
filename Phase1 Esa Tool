import streamlit as st
import openai

# Set your OpenAI API key
openai.api_key = st.secrets["OPENAI_API_KEY"]

st.set_page_config(page_title="Phase I ESA Assistant", layout="wide")
st.title("📋 Phase I ESA Proposal & Report Assistant")

st.markdown("Generate tailored proposals, scopes of work, and report summaries for Phase I Environmental Site Assessments.")

# --- Input Form ---
with st.form("esa_form"):
    st.header("Enter Site Details")
    col1, col2 = st.columns(2)
    with col1:
        project_name = st.text_input("Project Name", "Chula Vista Warehouse")
        site_address = st.text_input("Site Address", "1200 Industrial Blvd, Chula Vista, CA")
        site_type = st.selectbox("Site Type", ["Light Industrial", "Commercial", "Vacant Land", "Retail", "Office", "Mixed Use"])
        client_type = st.selectbox("Client Type", ["Private Investor", "Broker", "Bank/Lender", "Developer"])
        budget = st.number_input("Proposed Budget ($)", value=2600)
    with col2:
        historical_use = st.text_area("Historical Use", "Used for light manufacturing, then warehousing")
        known_issues = st.text_area("Known Issues", "Former UST, adjacent dry cleaner")
        intended_use = st.text_input("Intended Use", "Redevelopment (mixed-use)")
        timeline = st.selectbox("Turnaround Time", ["5 business days", "10 business days", "15 business days"])
        notes = st.text_area("Additional Notes", "Client prefers email delivery only")

    include_summary = st.checkbox("Include Executive Summary", True)
    include_proposal = st.checkbox("Include Proposal Email", True)
    include_scope = st.checkbox("Include Scope of Work", True)
    include_risks = st.checkbox("Include REC Risk Commentary", True)

    submitted = st.form_submit_button("Generate")

# --- GPT Prompt Function ---
def generate_output(prompt):
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are a professional environmental consultant who writes Phase I ESA documents and proposals."},
            {"role": "user", "content": prompt}
        ],
        temperature=0.6,
        max_tokens=900
    )
    return response.choices[0].message.content.strip()

# --- Output Section ---
if submitted:
    st.divider()
    st.subheader("Generated Content")

    context = f"""
    Project Name: {project_name}
    Site Address: {site_address}
    Site Type: {site_type}
    Historical Use: {historical_use}
    Known Issues: {known_issues}
    Intended Use: {intended_use}
    Client Type: {client_type}
    Budget: ${budget}
    Timeline: {timeline}
    Notes: {notes}
    """

    if include_proposal:
        with st.expander("📨 Proposal Email"):
            prompt = f"Using the following project info, write a professional but friendly proposal email to a client for a Phase I ESA: {context}"
            st.write(generate_output(prompt))

    if include_scope:
        with st.expander("📋 Scope of Work"):
            prompt = f"Write a Phase I ESA Scope of Work for the following site and context, using ASTM E1527-21 terminology but make it readable for non-technical clients: {context}"
            st.write(generate_output(prompt))

    if include_summary:
        with st.expander("📘 Executive Summary for Report"):
            prompt = f"Write an executive summary for a Phase I ESA report based on this project context. Keep it clear and professional: {context}"
            st.write(generate_output(prompt))

    if include_risks:
        with st.expander("⚠️ REC Risk Commentary"):
            prompt = f"Based on the site context, provide a summary of any potential Recognized Environmental Conditions (RECs), Historical RECs (HRECs), or Controlled RECs (CRECs) that might be of concern: {context}"
            st.write(generate_output(prompt))
