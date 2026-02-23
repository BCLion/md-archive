# Cell 7 - Ingestion Phase: Wormhole API Mastery
# This cell fetches the Wormhole Swagger and has the Architect define the "Smart Money" extraction plan.

# Cell 7 - Ingestion Phase: Wormhole API Mastery & KNF Strategy
import requests

swagger_url = "https://api.wormholescan.io/swagger.json"
swagger_doc = requests.get(swagger_url).json()

# Specific Intel from Wormhole Docs
WORMHOLE_INTEL = {
    "api_base": "https://api.wormholescan.io/api/v1",
    "solana_id": 1,
    "base_id": 30,
    "ethereum_id": 2,
    "vaa_template": "{chain}/{emitter}/{sequence}"
}

# Revised Instruction
knf_directive = f"""
Architect, use this Ground Truth: {WORMHOLE_INTEL}.
The Worker must NOT guess endpoints. To track Smart Money:
1. Fetch the latest VAA IDs for Solana (1) or Base (30).
2. Use the 'signed_vaa/{{vaa_id}}' endpoint to get 'vaaBytes'.
3. Apply the Koopman Neural Forecaster (KNF) to the 'amount' field in these bytes.
4. If the Worker doesn't have a transaction hash, it should first fetch 'observations' 
   to find recent emitter/sequence pairs.
"""

analysis_plan = ask_architect(f"{knf_directive}\n\nPlan the next move.")
print(analysis_plan)

# Cell 7 - Teaching the Agents to "Decode"
decoding_intel = """
Architect, the previous run failed because the 'amount' is hidden inside the 'vaaBytes' (Base64).
To get the real Smart Money data, the Worker must:
1. Fetch the 'signed_vaa/{vaa_id}'.
2. Decode the 'vaaBytes' from Base64.
3. The VAA Payload starts after the 100th byte (approximately). 
4. For 'Token Bridge' transfers, the 'Amount' is a 32-byte integer starting at offset 51 of the payload.
5. If the Worker cannot decode it, tell it to use 'https://api.wormholescan.io' 
   which often has pre-parsed 'content' fields with 'amount' and 'symbol'.
"""

analysis_plan = ask_architect(f"{decoding_intel}\n\nDraft a new strategy to extract REAL numbers for the KNF.")
print(analysis_plan)


# Cell 7 - Precision Swagger Ingestion
schema_intel = """
Architect, use the following Swagger intelligence for the /vaas endpoint:
- Path: /api/v1/vaas
- Structure: The response is a JSON object with a 'data' key containing a LIST of VAA objects.
- VAA Object Keys: 'txHash', 'emitterChain', 'emitterAddr', 'sequence', and 'payloadSummary'.
- Extraction Path: payloadSummary -> parsedPayload -> amount.
- Important: All amounts in VAAs are typically hex or large integers. If 'decimals' is found in AssetMeta, apply it.
"""
analysis_plan = ask_architect(f"{schema_intel}\n\nDraft a robust KNF script targeting /api/v1/vaas.")
print(analysis_plan)





# Cell 7 - Targeted Data Extraction
discovery_intel = """
Architect, the previous run got 0 data points because 'amount' is often nested or formatted as a string.
Instruct the Worker to:
1. Fetch 50 operations (limit=50) to increase the sample size.
2. For each record, check multiple paths for the amount: 
   - record.get('content', {}).get('standback', {}).get('amount')
   - record.get('content', {}).get('payload', {}).get('amount')
   - record.get('payload', {}).get('amount')
3. IMPORTANT: If 'amount' is a string like '100000000', convert it to float. 
4. Filter out any record where 'amount' is 0 or None.
5. If the Worker finds 'symbol', use it; otherwise, default to 'UNKNOWN'.
"""

analysis_plan = ask_architect(f"{discovery_intel}\n\nDraft the KNF extraction logic with these paths.")
print(analysis_plan)



# Cell 7 - The Recursive Hunter Intel
recursive_intel = """
Architect, the Worker is failing to 'see' the data. 
Instruct the Worker to use a Recursive Function to find 'amount'.
Logic:
1. Define a function `find_key(data, target_key)` that traverses lists and dicts.
2. Search for 'amount', 'value', or 'tokenAmount'.
3. If the value is a string, strip non-numeric chars and convert to float.
4. If 'decimals' exists in the same object, divide the amount by 10^decimals (e.g., amount/10^6 for USDC).
5. Target the 'https://api.wormholescan.io' endpoint specifically.
"""

analysis_plan = ask_architect(f"{recursive_intel}\n\nDraft the 'Universal Hunter' code for the Worker.")
print(analysis_plan)





# OLD BELOW # 
# We update the prompt to include the Google Research KNF requirement

#knf_directive = """
#Analyze this Swagger doc. Focus on VAA and token transfer endpoints for tracking 'Whales'. 
#CRITICAL: Integrate the Koopman Neural Forecaster (KNF) approach (inspired by Google Research). 
#Instruct the Worker to use Koopman operators to map non-linear liquidity flows into a linear latent space 
#for forecasting Smart Money movement between Solana (Chain 1) and Ethereum (Chain 2).
#Architect, use this Ground Truth: {WORMHOLE_INTEL}.
#The Worker must NOT guess endpoints. To track Smart Money:
#1. Fetch the latest VAA IDs for Solana (1) or Base (30).
#2. Use the 'signed_vaa/{{vaa_id}}' endpoint to get 'vaaBytes'.
#3. Apply the Koopman Neural Forecaster (KNF) to the 'amount' field in these bytes.
#4. If the Worker doesn't have a transaction hash, it should first fetch 'observations' 
#   to find recent emitter/sequence pairs.
#"""

#analysis_plan = ask_architect(f"{knf_directive}\n\nSwagger Summary: {str(swagger_doc)[:2000]}")
#print(f"🚀 Architect's Strategic Plan:\n{analysis_plan}")


