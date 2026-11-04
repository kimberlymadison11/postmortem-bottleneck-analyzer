import streamlit as st
import json
import os
from openai import OpenAI

# Page Configuration
st.set_page_config(
    page_title="AI Postmortem & Bottleneck Analyzer | Kimberly Madison",
    page_icon="🔍",
    layout="wide"
)

# Custom Styling
st.markdown("""
    <style>
    .main-header { font-family: 'Bricolage Grotesque', sans-serif; font-size: 2.2rem; font-weight: 800; color: #111827; }
    .sub-header { font-family: 'Source Serif 4', serif; font-size: 1.1rem; color: #4b5563; margin-bottom: 2rem; }
    </style>
""", unsafe_allow_html=True)

st.markdown('<div class="main-header">AI Cross-Functional Postmortem & Bottleneck Analyzer</div>', unsafe_allow_html=True)
st.markdown(
    '<div class="sub-header">Automated operational gap analysis and cross-functional delay triage for Enterprise Program Management.</div>',
    unsafe_allow_html=True
)

# Pre-loaded Sample Datasets
SAMPLE_POSTMORTEMS = {
    "Multi-Site ERP System Migration": """
    Project: Q3 Enterprise ERP Transformation Launch
    Target Go-Live: Sept 1 | Actual Go-Live: Oct 15 (6 weeks delayed)
    
    Notes & Feedback:
    - Engineering reported that legacy data schemas from the Shipwire acquisition had incomplete customer master records, causing 3 data migration script failures.
    - Operations noted warehouse leads were not trained on the new packing workflows until 3 days before original go-live, leading to inventory reconciliation halts.
    - Finance flagged that billing accuracy rules for custom pallet billing were missing from initial requirements specifications, requiring emergency code patches post-launch.
    - Legal review for vendor API data-sharing agreements delayed integration testing by 14 days due to contract redlines.
    - Sales escalated that 4 key accounts experienced SLA shipment delays during the cutover week due to lack of customer-facing status communications.
    """,
    "Global Client API Integration & Onboarding": """
    Project: Tier-1 Enterprise Merchant Platform Integration
    Target Go-Live: June 15 | Actual Go-Live: July 30 (45 days delayed)
    
    Notes & Feedback:
    - Client engineering team repeatedly failed sandbox validation due to unclear REST API endpoint documentation provided by our Product team.
    - CSM team was omitted from weekly technical syncs, resulting in client leadership escalating friction to our CEO.
    - Security audit uncovered missing OAuth token refresh parameters two days prior to scheduled cutover.
    - No clear escalation path was defined between 3rd-party logistics partners and our internal support team.
    """
}

# Sidebar
st.sidebar.header("Program Selection")
selected_title = st.sidebar.selectbox("Select a Sample Implementation Postmortem:", list(SAMPLE_POSTMORTEMS.keys()))

st.subheader(f"Implementation Retro Notes: {selected_title}")
raw_notes = st.text_area("Unstructured Retro & Postmortem Notes", value=SAMPLE_POSTMORTEMS[selected_title], height=220)

st.divider()

openai_api_key = st.sidebar.text_input("OpenAI API Key (Optional for live generation)", type="password")

def analyze_postmortem(notes, api_key=None):
    if api_key:
        try:
            client = OpenAI(api_key=api_key)
            prompt = f"""
            You are an AI Enterprise Program Manager assisting Program Director Kimberly Madison.
            Analyze the following unstructured project postmortem notes and extract structured insights.

            POSTMORTEM NOTES:
            {notes}

            OUTPUT STRUCTURED SECTIONS:
            1. ROOT CAUSE MATRIX (Categorize bottlenecks by Team: Engineering, Operations, Finance, Product, Legal, Sales).
            2. SLA & TIMELINE IMPACT SUMMARY (Quantify delay drivers and business impact).
            3. CORRECTIVE ACTION TRACKER (Structured 30-60-90 Day Action items assigned to owners).
            """
            response = client.chat.completions.create(
                model="gpt-4o",
                messages=[
                    {"role": "system", "content": "You excel at program management, operational gap analysis, and cross-functional leadership execution."},
                    {"role": "user", "content": prompt}
                ],
                temperature=0.3
            )
            return response.choices[0].message.content
        except Exception as e:
            st.error(f"OpenAI error: {str(e)}. Displaying pre-computed analysis.")

    # Pre-computed Demo Output
    return """### 1. Cross-Functional Root Cause Matrix
* **Engineering & Architecture:** Legacy data schema incompatibilities delayed data migration scripts by 3 weeks. Missing automated validation prior to cutover.
* **Operations & Change Management:** Warehouse enablement occurred too late in the lifecycle (3 days prior to go-live), leading to inventory reconciliation halts.
* **Product & Systems Analysis:** Missing initial requirements for custom billing rules; required emergency patch release.
* **Legal & Governance:** Vendor API contract redlines added 14 days of unexpected block time to integration testing.
* **Customer Success & GTM:** Lack of external communication playbooks resulted in client escalations during cutover.

---

### 2. SLA & Timeline Impact Summary
* **Total Project Delay:** 6 Weeks (45 Days).
* **Primary Schedule Driver:** Legacy data cleanup & uncoordinated legal review (35 days total critical path delay).
* **Business & Revenue Impact:** Operational shipping SLA degradation across 4 enterprise accounts; temporary billings hold due to missing ERP billing logic.

---

### 3. 30-60-90 Day Corrective Action Plan
* **30 Days (Immediate Triage):** Standardize API legal redline templates with Legal; implement mandatory data-cleansing pre-checks for future site migrations.
* **60 Days (Process & Training):** Mandate warehouse operational readiness training 21 days prior to any system cutover; establish formal GTM escalation protocols.
* **90 Days (Governance & Systems):** Institute formal cross-functional sign-offs (Eng, Ops, Fin, Product) before locking project scope and launch dates.
"""

if st.button("🚀 Run Postmortem Bottleneck Analyzer", type="primary"):
    with st.spinner("Synthesizing postmortem feedback and generating action plan..."):
        analysis = analyze_postmortem(raw_notes, openai_api_key)
        
        tab1, tab2, tab3 = st.tabs(["📊 Root Cause Matrix", "⏱️ SLA & Timeline Impact", "🗓️ 30-60-90 Day Action Tracker"])
        sections = analysis.split("---")
        
        with tab1:
            st.markdown(sections[0] if len(sections) > 0 else analysis)
        with tab2:
            st.markdown(sections[1] if len(sections) > 1 else "Timeline impact analysis generated.")
        with tab3:
            st.markdown(sections[2] if len(sections) > 2 else "Action plan generated.")
