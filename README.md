# 🛡️ Fair Lending AI Assistant: Proactive Bias Detection in Credit Decisions

## Empowering UK Financial Institutions with Transparent & Ethical AI Lending

**Project Maintainer:** Samlytics Data Service
**Contact:** contactus@samlytics.co.uk

---

### Introduction

In an increasingly data-driven world, Artificial Intelligence (AI) offers unparalleled opportunities for efficiency and accuracy in financial services. However, the responsible adoption of AI, particularly in sensitive areas like lending, is paramount. This project, "Fair Lending AI Assistant," demonstrates a practical and ethical application of AI to help UK financial institutions proactively identify and mitigate unfair lending practices.

Leveraging **Explainable AI (XAI)** and built on **Google Cloud Platform (GCP)**, this solution provides not just predictions, but transparent insights into *why* a loan decision was made. This empowers financial institutions to uphold fairness, comply with evolving UK regulatory standards (such as those from the FCA and the UK Government's AI Regulation White Paper on fairness, transparency, and accountability), and build stronger trust with their customers.

### The Challenge of Unfair Lending

Traditional lending models, or even poorly implemented AI systems, can inadvertently perpetuate historical biases present in training data. This can lead to:
* **Disparate Impact:** Certain demographic groups (e.g., based on gender, age, residential area) receiving disproportionately unfavourable loan outcomes.
* **Lack of Transparency:** Inability to explain why a loan was approved or rejected, leading to customer distrust and regulatory scrutiny.
* **Regulatory Risk:** Non-compliance with anti-discrimination laws and financial regulations, leading to fines and reputational damage.

### Our Solution: AI-Powered Fairness & Transparency

This project provides a robust framework to address these challenges:

1.  **Bias-Aware Model Training:** Our machine learning model is trained on a synthetic dataset designed to reflect common biases, allowing the model to learn to identify patterns that might indicate unfairness.
2.  **Explainable AI (XAI) Integration:** Using Vertex AI's integrated explanation capabilities (like Integrated Gradients), the system pinpoints the contribution of each input feature (e.g., credit score, income, but also sensitive attributes like gender or residential area) to the final loan decision.
3.  **Interactive Human-in-the-Loop Validation:** A user-friendly web application allows loan officers and compliance teams to:
    * Input hypothetical loan applications.
    * Receive an instant AI-driven decision (Approved/Rejected).
    * Crucially, view a detailed breakdown of the *factors influencing the decision*.
    * Identify if sensitive attributes might be unduly affecting the outcome, prompting a manual review.

### How it Works (Technical Overview)

* **Data:** A synthetic dataset (`synthetic_loan_data.csv`) is generated, mimicking real-world loan applications and intentionally embedding subtle biases related to `gender` and `residential_area_type`.
* **Machine Learning Model:** A scikit-learn Logistic Regression model (chosen for its interpretability and efficiency) is trained on the preprocessed data.
* **Google Cloud Vertex AI:**
    * The dataset is stored in **Google Cloud Storage (GCS)**.
    * The model is trained and managed using **Vertex AI Model Registry**.
    * The trained model is deployed to a **Vertex AI Endpoint** for real-time predictions.
    * **Vertex AI Explainable AI** is configured to provide feature attributions for each prediction, making the "black box" transparent.
* **Interactive Application (Streamlit):** A Python Streamlit web application (`app.py`) serves as the front-end, allowing users to interact with the deployed AI model via the Vertex AI Endpoint.

### Benefits for UK Financial Institutions

* **Enhanced Regulatory Compliance:** Proactively address requirements for fairness, transparency, and accountability set by the FCA and the UK Government's AI principles.
* **Reduced Bias:** Systematically identify and mitigate algorithmic biases, leading to more equitable lending outcomes for all customers.
* **Increased Trust & Reputation:** Build confidence with customers by providing clear, understandable reasons for loan decisions.
* **Improved Decision-Making:** Empower loan officers with AI-driven insights, while retaining human oversight for complex or ethically sensitive cases.
* **Operational Efficiency:** Automate initial assessments while flagging high-risk or potentially unfair decisions for human review.
* **Scalability & Security:** Leverage GCP's robust and secure infrastructure for scalable AI deployment and data management.

### Interactive Use Case Samples (Try it yourself!)

To demonstrate the power of this system, consider these scenarios:

1.  **Scenario 1: Identifying Subtle Gender Bias**
    * Input: A "Male" applicant with a good credit score (e.g., 700), good income (£4000), 10 years employment, requesting £20,000.
    * Observe the AI decision and explanations.
    * Now, change only the `Gender` to "Female", keeping all other details identical.
    * **Expected Observation:** In a biased system (like our synthetic one), you might see the "Female" applicant's probability of approval slightly decrease, or the "Gender" feature might show a small negative contribution, prompting review.

2.  **Scenario 2: Area-Based Discrimination**
    * Input: An applicant from an "Urban" area with average metrics (e.g., credit score 600, income £3500), requesting £15,000.
    * Observe the AI decision and explanations.
    * Change only the `Residential Area Type` to "Rural", keeping other details identical.
    * **Expected Observation:** Similar to the gender bias, the "Rural" applicant might experience a slight negative shift in probability or the 'Residential Area Type' feature shows a negative influence, highlighting a potential issue.

3.  **Scenario 3: Validating Fair Decisions**
    * Input: A high credit score (e.g., 800), high income (£6000), no previous defaults.
    * Observe that the loan is likely approved with strong positive contributions from `credit_score` and `income`. The sensitive attributes should have minimal impact on the positive decision.

These interactive scenarios provide a tangible way for financial institutions to engage with and validate the AI's outcomes, fostering a data-driven approach to ethical lending.

### Getting Started (For Developers/IT Teams)

To run this project locally or deploy it within your GCP environment:

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/SamlyticsDS/unfair-lending-detector](https://github.com/SamlyticsDS/unfair-lending-detector)
    
    cd unfair-lending-detector
    ```
2.  **Set up Google Cloud:**
    * Ensure you have a GCP Project and billing enabled.
    * Enable the `Vertex AI API` and `Cloud Storage API`.
    * Install and authenticate `gcloud CLI`: `gcloud init`, `gcloud auth application-default login`.
    * Create a service account with `Vertex AI User` and `Storage Object Admin` roles and set it as your default for API calls.
3.  **Install Python Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    (Create `requirements.txt` with: `pandas`, `numpy`, `scikit-learn`, `google-cloud-aiplatform`, `google-cloud-storage`, `streamlit`, `joblib`)
4.  **Generate Synthetic Data:**
    Run `python generate_synthetic_data.py` (or integrate the data generation into the training script as shown in `train_deploy_model.py`).
5.  **Train & Deploy the Model to Vertex AI:**
    * **IMPORTANT:** Update `PROJECT_ID` in `train_deploy_model.py` to your GCP Project ID.
    * Run: `python train_deploy_model.py`
    * This script will upload data, train the model, and deploy it to a Vertex AI Endpoint with XAI enabled. Note down the `ENDPOINT_ID` displayed in the output.
6.  **Run the Interactive Streamlit App:**
    * **IMPORTANT:** Update `PROJECT_ID` and `ENDPOINT_ID` in `app.py` to match your GCP setup.
    * Run: `streamlit run app.py`
    * This will open the web application in your browser.

### Recommendations for Financial Institutions

1.  **Pilot Programme:** Start with a pilot programme on a subset of loan applications, running decisions in parallel with human-made ones and comparing outcomes.
2.  **Continuous Monitoring:** Implement robust monitoring frameworks for your AI models (e.g., Vertex AI Model Monitoring) to detect model drift and shifts in fairness metrics over time.
3.  **Human Oversight:** Maintain and empower human loan officers and compliance teams. AI should be an **assistant**, not a replacement, especially in high-stakes decisions like lending.
4.  **Data Quality & Diversity:** Invest continuously in improving the quality, representativeness, and diversity of your training data to minimise inherent biases.
5.  **Ethical AI Governance:** Establish clear internal guidelines, ethics committees, and regular audits for all AI systems, particularly those impacting customers. Refer to the UK's AI regulatory principles (safety, security, robustness, transparency, explainability, fairness, accountability, governance, contestability, and redress).
6.  **Regular Model Retraining:** Regularly retrain models with fresh data to adapt to changing market conditions and societal norms.
7.  **Training & Upskilling:** Train your staff on AI concepts, fairness principles, and how to effectively use XAI tools to interpret and validate AI decisions.

### Contributing

We welcome contributions to enhance this project! Please feel free to fork the repository, open issues, or submit pull requests.

---

**Disclaimer:** This project uses synthetic data and a simplified model for demonstration purposes. Real-world financial applications require highly robust, audited, and continuously validated models with extensive regulatory compliance checks. Always consult with legal and compliance experts when deploying AI in regulated environments.
