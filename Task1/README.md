import pandas as pd
from transformers import pipeline

# -----------------------------
# 1. Load Dataset
# -----------------------------
file_path = "Superstore.xlsx"  # Change path if needed
df = pd.read_excel(file_path)

# Remove unnecessary columns if present
for col in ["ind1", "ind2"]:
    if col in df.columns:
        df.drop(columns=[col], inplace=True)

# Convert date columns if present
for date_col in ["Order Date", "Ship Date"]:
    if date_col in df.columns:
        df[date_col] = pd.to_datetime(df[date_col], errors="coerce")

# -----------------------------
# 2. Load Open-source LLM
# -----------------------------
chatbot = pipeline("text-generation", model="gpt2")

# -----------------------------
# 3. Helper function for dataset queries
# -----------------------------
def answer_dataset_query(query):
    query_lower = query.lower()

    if "top 5 countries" in query_lower:
        if "Country" in df.columns:
            result = df.groupby("Country")["Sales"].sum().nlargest(5)
            return str(result)
    
    elif "total profit" in query_lower:
        total_profit = df["Profit"].sum()
        return f"Total Profit: {total_profit:,.2f}"
    
    elif "best-selling category" in query_lower:
        if "Category" in df.columns:
            best_cat = df.groupby("Category")["Sales"].sum().idxmax()
            return f"Best-selling category: {best_cat}"
    
    elif "sales trend" in query_lower:
        if "Order Date" in df.columns:
            sales_trend = df.groupby(df["Order Date"].dt.to_period("M"))["Sales"].sum().head()
            return str(sales_trend)
    
    elif "top 10 customers" in query_lower:
        if "Customer Name" in df.columns:
            result = df.groupby("Customer Name")["Sales"].sum().nlargest(10)
            return str(result)
    
    return None

# -----------------------------
# 4. Chat Loop
# -----------------------------
print("Ask your question (type 'exit' to stop):")

while True:
    user_input = input("You: ")
    if user_input.lower() == "exit":
        print("Chatbot: Goodbye!")
        break

    # First check if it's a dataset query
    dataset_answer = answer_dataset_query(user_input)
    if dataset_answer:
        print(f"Chatbot (Data Insight):\n{dataset_answer}")
    else:
        # Otherwise use GPT-2 for generic answer
        response = chatbot(user_input, max_length=100, truncation=True)
        print(f"Chatbot: {response[0]['generated_text']}")
