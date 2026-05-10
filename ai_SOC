from flask import Flask, request, jsonify
import ollama
import json
import datetime

app = Flask(__name__)
# This function sends the data to the log file Wazuh is watching
def log_to_wazuh(prompt, response, remote_ip):
    log_entry = {
        "timestamp": datetime.datetime.now().isoformat(),
        "application": "ollama_ai",
        "user_input": prompt,
        "ai_output": response,
        "srcip": remote_ip
    }
    # We use 'a' to append so we don't delete old logs
    with open("/var/log/ai_security.log", "a") as f:
        f.write(json.dumps(log_entry) + "\n")

# 2. This creates the "Door" that the other VM will knock on
@app.route('/chat', methods=['GET'])
def chat():
    # Grab the prompt from the URL (?prompt=whatever)
    user_prompt = request.args.get('prompt')
    
    # Grab the ACTUAL IP of the Attacker VM
    attacker_ip = request.remote_addr 
    
    # Get Llama 3's brain moving
    response = ollama.generate(model='llama3', prompt=user_prompt)
    ai_text = response['response']
    
    # Log the interaction with the forensic IP
    log_to_wazuh(user_prompt, ai_text, attacker_ip)
    
    return jsonify({"ai_response": ai_text})

if __name__ == '__main__':
    # Starts the server on port 5000
    print("--- AI SOC NETWORK GATEWAY: ACTIVE ---")
    app.run(host='0.0.0.0', port=5000)
