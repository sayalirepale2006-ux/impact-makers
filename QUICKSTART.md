# 🚀 Quick Start Guide - Credit Risk Assessment API

## Prerequisites

- Python 3.8+
- MongoDB installed and running
- pip (Python package manager)

## Setup in 5 Steps

### 1️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

### 2️⃣ Start MongoDB

Make sure MongoDB is running on `localhost:27017` (default port)

**Windows:**
```bash
# If installed as service, it should be running automatically
# Or start manually:
mongod
```

**Mac/Linux:**
```bash
# Using brew (Mac)
brew services start mongodb-community

# Or manually
mongod --config /usr/local/etc/mongod.conf
```

### 3️⃣ Configure Environment (Optional)

```bash
# Copy environment template
cp .env.example .env

# Edit .env if needed (default works for local setup)
```

### 4️⃣ Run the Server

```bash
# Start FastAPI server
uvicorn app.main:app --reload

# Or using Python
python -m app.main
```

**Output:**
```
INFO:     Uvicorn running on http://127.0.0.1:8000
INFO:     Application startup complete.
✅ Connected to MongoDB
```

### 5️⃣ Test the API

**Option A: Interactive Swagger UI**

Visit: `http://localhost:8000/docs`

**Option B: Run Test Script**

```bash
python test_api.py
```

**Option C: Manual cURL**

```bash
# Register a user
curl -X POST "http://localhost:8000/register" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "job_type": "Delivery Driver",
    "months_active": 18
  }'

# Calculate score (replace USER_ID with actual ID from registration)
curl -X POST "http://localhost:8000/calculate-score" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "USER_ID",
    "avg_income": 25000,
    "income_variance": 0.2,
    "upi_txn_count": 45,
    "bill_payment_score": 8,
    "withdrawal_ratio": 0.5
  }'
```

## 🎯 Expected Results

### Low Risk Example (Score: ~75)
```json
{
  "digital_trust_score": 75,
  "risk_category": "Low Risk",
  "explanation": [
    "Stable income pattern detected",
    "High UPI transaction activity",
    "Regular bill payments recorded",
    "Long-term work activity"
  ]
}
```

### Medium Risk Example (Score: ~50)
```json
{
  "digital_trust_score": 50,
  "risk_category": "Medium Risk",
  "explanation": [
    "Income fluctuation detected",
    "Moderate UPI activity",
    "Occasional bill payments"
  ]
}
```

### High Risk Example (Score: ~25)
```json
{
  "digital_trust_score": 25,
  "risk_category": "High Risk",
  "explanation": [
    "Income fluctuation detected",
    "Low digital payment activity",
    "Irregular bill payment history",
    "High cash withdrawal behavior"
  ]
}
```

## 📊 API Endpoints Summary

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/register` | Register new user |
| POST | `/calculate-score` | Calculate credit score |
| GET | `/users` | Get all users |
| GET | `/user/{id}` | Get user with credit profile |
| GET | `/health` | Health check |
| GET | `/docs` | API documentation |

## 🔧 Troubleshooting

### MongoDB Connection Error
```
Error: Could not connect to MongoDB
```
**Solution:** Make sure MongoDB is running on port 27017

### Import Error
```
ModuleNotFoundError: No module named 'fastapi'
```
**Solution:** Install dependencies: `pip install -r requirements.txt`

### Port Already in Use
```
Error: Address already in use
```
**Solution:** Change port: `uvicorn app.main:app --port 8001 --reload`

## 📁 Project Structure

```
Hackathon project/
├── app/
│   ├── main.py              # FastAPI app
│   ├── database.py          # MongoDB setup
│   ├── models.py            # Database models
│   ├── schemas.py           # Request/Response schemas
│   ├── scoring.py           # Scoring logic
│   ├── routes/
│   │   ├── users.py         # User routes
│   │   └── credit.py        # Credit routes
│   └── ml/
│       └── model.py         # ML placeholder
├── test_api.py              # Test script
├── requirements.txt         # Dependencies
├── README.md                # Full documentation
└── QUICKSTART.md           # This file
```

## 🎓 Next Steps

1. ✅ Server is running
2. ✅ API is tested
3. 🚀 Build your React frontend
4. 🔗 Connect frontend to these endpoints
5. 🎨 Add authentication (JWT)
6. 📊 Add analytics dashboard

## 📚 Full Documentation

For detailed information, see [README.md](README.md)

---

**Happy Hacking! 🎉**
