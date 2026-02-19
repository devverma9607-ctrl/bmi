# bmi_calculator.py
import streamlit as st

# Configure page settings
st.set_page_config(
    page_title="BMI Calculator",
    page_icon="⚖️",
    layout="centered"
)

# Custom CSS for styling
st.markdown("""
<style>
    .stNumberInput > div > div > input {
        background-color: #f0f2f6;
    }
    .stButton button {
        background-color: #4CAF50 !important;
        color: white !important;
    }
    .bmi-result {
        font-size: 1.2rem;
        padding: 1rem;
        border-radius: 10px;
        text-align: center;
        margin-top: 1.5rem;
    }
</style>
""", unsafe_allow_html=True)

# Title and description
st.title("⚖️ BMI Calculator")
st.subheader("Calculate Your Body Mass Index")
st.write("BMI is a simple index of weight-for-height commonly used to classify underweight, overweight, and obesity.")

# Measurement system selection
system = st.radio("Select measurement system:", ("Metric (kg/cm)", "Imperial (lb/in)"))

# Input fields based on selected system
with st.form("bmi_form"):
    if system == "Metric (kg/cm)":
        weight = st.number_input("Weight (kg)", min_value=1.0, max_value=300.0, value=70.0)
        height = st.number_input("Height (cm)", min_value=50.0, max_value=250.0, value=170.0)
    else:
        weight = st.number_input("Weight (lb)", min_value=2.2, max_value=660.0, value=150.0)
        height = st.number_input("Height (in)", min_value=20.0, max_value=100.0, value=65.0)
    
    submitted = st.form_submit_button("Calculate BMI")

# BMI calculation function
def calculate_bmi(weight, height, system):
    if system == "Metric (kg/cm)":
        height_m = height / 100
        bmi = weight / (height_m ** 2)
    else:
        bmi = (weight / (height ** 2)) * 703
    return round(bmi, 1)

# Interpretation function
def interpret_bmi(bmi):
    if bmi < 18.5:
        category = "Underweight"
        color = "lightblue"
    elif 18.5 <= bmi < 25:
        category = "Normal weight"
        color = "lightgreen"
    elif 25 <= bmi < 30:
        category = "Overweight"
        color = "orange"
    else:
        category = "Obese"
        color = "lightcoral"
    return category, color

# Calculate and display results
if submitted:
    bmi = calculate_bmi(weight, height, system)
    category, color = interpret_bmi(bmi)
    
    st.divider()
    st.subheader("Results")
    
    # Create columns for layout
    col1, col2 = st.columns(2)
    
    with col1:
        st.metric("Your BMI", f"{bmi} kg/m²")
    
    with col2:
        st.markdown(f"<div class='bmi-result' style='background-color:{color}'>"
                    f"<strong>Category:</strong> {category}</div>", 
                    unsafe_allow_html=True)
    
    # Reference table
    st.write("### BMI Categories:")
    st.table({
        "Category": ["Underweight", "Normal weight", "Overweight", "Obese"],
        "BMI Range": ["< 18.5", "18.5 - 24.9", "25.0 - 29.9", "≥ 30.0"]
    })
    
    # Disclaimer
    st.caption("Note: BMI is a screening tool but not diagnostic of body fatness or health. Consult a healthcare provider for assessment.")

# Run instructions sidebar
with st.sidebar:
    st.header("How to Run")
    st.write("1. Install requirements:")
    st.code("pip install streamlit")
    st.write("2. Run the app:")
    st.code("streamlit run bmi_calculator.py")
    st.write("3. Open browser to `localhost:8501`")
    st.divider()
    st.write("💡 **Tip:** Use the hamburger menu to rerun or clear inputs")
