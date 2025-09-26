📊 Volume Bot (Python)

A FastAPI-based Solana analytics & trading assistant.
The bot fetches on-chain liquidity pool state data, parses AMM configurations, tracks vault balances, and stores buy/sell transaction history in a database.

✨ Features

⚡ Solana blockchain integration via solders client.

🔍 Liquidity Pool State Parser:

Fetch pool config, vault balances, LP supply, fees, and observations.

Decode Raydium-style AMM state layouts.

📡 Observation Oracle: Retrieve cumulative token prices and time-weighted observations.

🗄 Database-backed transaction history (SQLAlchemy ORM).

🛠 REST API (FastAPI) for external integrations.

🔑 Modular architecture for easy extension.


⚙️ Installation

Clone the repository:

git clone https://github.com/ptechfusion19/volume-bot-py.git
cd volume-bot-py


Create and activate a virtual environment:

python3 -m venv venv
source venv/bin/activate   # Linux / Mac
venv\Scripts\activate      # Windows


Install dependencies:

pip install -r requirements.txt

🚀 Running the Project

Start the FastAPI app:

uvicorn main:app --reload


API will be available at:
👉 http://127.0.0.1:8000

Interactive Swagger docs:
👉 http://127.0.0.1:8000/docs

🧩 Core Components
🔹 PoolState

Defined in state.py, it:

Fetches on-chain pool state via Solana RPC.

Parses AMM configuration, vault balances, LP mint details.

Retrieves observation oracle data (cumulative prices).

Calculates vault balances with/without fees.

Example:

from state import PoolState
from solders.rpc import RpcClient

client = RpcClient("https://api.mainnet-beta.solana.com")
pool = PoolState("<POOL_STATE_ADDRESS>", client)

print(pool.vault_amout)
print(pool.amm_cfg)

🔹 Database Transactions

Located in database/models/transaction.py + CRUD functions.

create_transaction() → Add a new buy/sell.

get_transaction() → Fetch single transaction.

get_transactions() → Fetch list.

update_transaction() → Update fields.

delete_transaction() → Remove entry.

Example:

from sqlalchemy.orm import Session
from database.crud.transaction import create_transaction

db: Session = ...
txn = create_transaction(
    db, wallet_id=1, task_id=42,
    transaction_sig="5z7..abc",
    amount=100.5, transaction_type="buy"
)
print(txn.id, txn.amount)

🔹 API Routes

Routers are defined in api/routes/router.py.
Endpoints expose pool and transaction data for integrations.

Example (Swagger UI):

GET /transactions
POST /transactions
PUT /transactions/{id}
DELETE /transactions/{id}

📊 Example Use Case

Initialize bot with a pool address.

Bot fetches on-chain state (vault balances, LP supply, fees).

Observations are retrieved for price calculations.

Buy/sell activity is stored in the SQL database.

REST API makes this data available to dashboards or other services.
