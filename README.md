# Credit Risk Assessment API

Ethical & Explainable Credit Risk Assessment System for Informal and Gig Workers

## 🚀 Features

- **User Registration**: Register gig workers with job details
- **Digital Trust Score**: Calculate credit scores based on alternative financial indicators
- **Risk Classification**: Classify users into Low/Medium/High risk categories
- **Explainable AI**: Generate human-readable explanations for scores
- **MongoDB Storage**: Persist all user and credit data
- **RESTful API**: Clean FastAPI endpoints for frontend integration

## 📋 Tech Stack

- **Backend Framework**: FastAPI (Python)
- **Database**: MongoDB
- **MongoDB Driver**: Motor (async)
- **Data Validation**: Pydantic
- **ML Library**: Scikit-learn (placeholder)

## 🏗️ Project Structure

```
app/
 ├── main.py              # FastAPI application entry point
 ├── database.py          # MongoDB connection
 ├── models.py            # Pydantic models for MongoDB
 ├── schemas.py           # Request/Response schemas
 ├── scoring.py           # Scoring & explainability logic
 ├── routes/
 │    ├── users.py        # User management routes
 │    └── credit.py       # Credit scoring routes
 └── ml/
      └── model.py        # ML model placeholder
```

## 🔧 Installation

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Set Up MongoDB

Make sure MongoDB is running locally on port 27017, or update the connection string in `.env` file.

```bash
# Copy environment template
cp .env.example .env

# Edit .env with your MongoDB URL
MONGODB_URL=mongodb://localhost:27017
```

### 3. Run the Application

```bash
# Development mode with auto-reload
uvicorn app.main:app --reload

# Or use Python directly
python -m app.main
```

The API will be available at: `http://localhost:8000`

Interactive API docs at: `http://localhost:8000/docs`

## 📡 API Endpoints

### 1️⃣ Register User

**POST** `/register`

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "job_type": "Delivery Driver",
  "months_active": 18
}
```

**Response:**
```json
{
  "id": "60d5ec49f1b2c8b1f8e4e1a1",
  "name": "John Doe",
  "email": "john@example.com",
  "job_type": "Delivery Driver",
  "months_active": 18,
  "created_at": "2024-02-16T10:30:00"
}
```

### 2️⃣ Calculate Score

**POST** `/calculate-score`

```json
{
  "user_id": "60d5ec49f1b2c8b1f8e4e1a1",
  "avg_income": 25000.0,
  "income_variance": 0.2,
  "upi_txn_count": 45,
  "bill_payment_score": 8,
  "withdrawal_ratio": 0.5
}
```

**Response:**
```json
{
  "user_id": "60d5ec49f1b2c8b1f8e4e1a1",
  "digital_trust_score": 75,
  "risk_category": "Low Risk",
  "explanation": [
    "Stable income pattern detected with low variance",
    "High UPI transaction activity observed - strong digital footprint",
    "Regular bill payments recorded - demonstrates financial discipline",
    "Long-term work activity improves trust and stability",
    "Low withdrawal ratio indicates good digital transaction habits"
  ],
  "credit_profile_id": "60d5ec49f1b2c8b1f8e4e1a2"
}
```

### 3️⃣ Get All Users

**GET** `/users`

Returns array of all registered users.

### 4️⃣ Get User Details

**GET** `/user/{id}`

Returns user details with latest credit profile.

## 🎯 Scoring Logic

The Digital Trust Score (0-100) is calculated using rule-based logic:

| Criteria | Condition | Points |
|----------|-----------|--------|
| Stable Income | income_variance < 0.3 | +25 |
| High UPI Activity | upi_txn_count > 30 | +20 |
| Regular Bill Payments | bill_payment_score > 7 | +20 |
| Work Duration | months_active ≥ 12 | +25 |
| High Withdrawals | withdrawal_ratio > 0.7 | -10 |

## 🏷️ Risk Classification

- **Score ≥ 70**: Low Risk
- **Score 40-69**: Medium Risk
- **Score < 40**: High Risk

## 💡 Explainability

Every score comes with human-readable explanations:

- "Stable income pattern detected"
- "High UPI transaction activity observed"
- "Regular bill payments recorded"
- "Long-term work activity improves trust"
- "High cash withdrawal behavior increases risk"

## 🤖 ML Module (Optional)

The `app/ml/model.py` module provides a placeholder for future ML integration:

- Random Forest Classifier
- Logistic Regression
- Feature importance analysis
- Model persistence

**Note**: Currently not used in production; rule-based scoring is active.

## 📊 MongoDB Schema

### `users` Collection

```javascript
{
  _id: ObjectId,
  name: String,
  email: String,
  job_type: String,
  months_active: Number,
  created_at: DateTime
}
```

### `credit_profiles` Collection

```javascript
{
  _id: ObjectId,
  user_id: String,
  avg_income: Number,
  income_variance: Number,
  upi_txn_count: Number,
  bill_payment_score: Number,
  withdrawal_ratio: Number,
  digital_trust_score: Number,
  risk_category: String,
  explanation: [String],
  created_at: DateTime
}
```

## 🧪 Testing

### Using cURL

```bash
# Register user
curl -X POST "http://localhost:8000/register" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Smith",
    "email": "jane@example.com",
    "job_type": "Freelance Designer",
    "months_active": 24
  }'

# Calculate score
curl -X POST "http://localhost:8000/calculate-score" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "YOUR_USER_ID",
    "avg_income": 30000,
    "income_variance": 0.15,
    "upi_txn_count": 50,
    "bill_payment_score": 9,
    "withdrawal_ratio": 0.3
  }'

# Get all users
curl "http://localhost:8000/users"

# Get specific user
curl "http://localhost:8000/user/YOUR_USER_ID"
```

### Using Python Requests

```python
import requests

# Register user
response = requests.post(
    "http://localhost:8000/register",
    json={
        "name": "Test User",
        "email": "test@example.com",
        "job_type": "Gig Worker",
        "months_active": 15
    }
)
user = response.json()
print(f"Registered user: {user['id']}")

# Calculate score
score_response = requests.post(
    "http://localhost:8000/calculate-score",
    json={
        "user_id": user["id"],
        "avg_income": 22000,
        "income_variance": 0.25,
        "upi_txn_count": 35,
        "bill_payment_score": 7,
        "withdrawal_ratio": 0.6
    }
)
result = score_response.json()
print(f"Score: {result['digital_trust_score']}")
print(f"Risk: {result['risk_category']}")
print("Explanations:")
for exp in result['explanation']:
    print(f"  - {exp}")
```

## 🌐 CORS Configuration

CORS is enabled for all origins in development. For production, update `app/main.py`:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://your-frontend-domain.com"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## 🔒 Security Notes

For production deployment:

1. Use environment variables for sensitive data
2. Enable MongoDB authentication
3. Add rate limiting
4. Implement JWT authentication
5. Use HTTPS
6. Validate all inputs strictly

## 📝 License

This project is for hackathon purposes and educational use.

## 🤝 Contributing

This is a hackathon MVP. Feel free to extend with:

- JWT authentication
- Advanced ML models
- Additional financial indicators
- Batch processing
- Analytics dashboard

---

**Built with ❤️ for ethical credit assessment**
