# ffuf

| Options                                                    | Description                                                              |
| ---------------------------------------------------------- | ------------------------------------------------------------------------ |
| **`-w`**                                                   | Provide wordlists                                                        |
| **`-u`**                                                   | Provide URL. The ffuf tool fuzzes at the `FUZZ` keyword.                 |
| **`-H`**                                                   | Provide Header value separated by colon. Multiple -H flags are accepted. |
| **`-b`**                                                   | Cookie data.                                                             |
| **`-X`**                                                   | HTTP method to use.                                                      |
| **`-ic`**                                                  | It is used to ignore the comments in the wordlist.                       |
| **`-e`**                                                   | Specify extension. E.g., `-e .php`.                                      |
| `-request`                                                 | Specify a request file to it.                                            |
| `-d`                                                       | Add data                                                                 |
| **Filter Options \[Removes filtered things from results]** |                                                                          |
| **`-fc`**                                                  | Filter HTTP status code from response.                                   |
| **`-fl`**                                                  | Filter by amount of lines in response.                                   |
| **`-fr`**                                                  | Filter regexp                                                            |
| **`-fs`**                                                  | Filter HTTP Response size.                                               |
| **`-fw`**                                                  | Filter by amount of words in response.                                   |
| **Matcher Options \[Appends matching option to results]**  |                                                                          |
| `-mc`                                                      | Match HTTP Status codes.                                                 |
| `-ml`                                                      | Match amount of lines in response                                        |
| `-mr`                                                      | Match regexp                                                             |
| `-ms`                                                      | Match HTTP response size                                                 |
| `-mw`                                                      | Match amount of words in response                                        |

## Directory Fuzzing

```bash
ffuf -w <wordlist> -u https://target.com/FUZZ -ic -t 200 -c
```

## Extension Fuzzing

**Check on which utility the site is running.** The extensions must contain `.`

```bash
ffuf -w <wordlist> -u http://target.com/blog/indexFUZZ -ic -t 200 -c
```

## Page Fuzzing

```bash
ffuf -w <wordlist> -u https://target.com/blog/FUZZ.php -ic -c -t 200
```

## Recursive Fuzzing

Above we first fuzz for directories and then fuzzing for pages under these directories. But using recursive fuzzing we can fuzz for both.

In recursive fuzzing if we specify the `-recursion-depth 1`, it will only fuzz the main directories and their direct sub-directories. We can specify extension with `-e .php`.

{% code overflow="wrap" %}
```bash
ffuf -w <wordlist> -u https://target.com/FUZZ -recursion -recursion-depth 1 -e .php -v -ic -c
```
{% endcode %}

## Subdomain Fuzzing

```bash
ffuf -w <wordlist> -u https://FUZZ.target.com/ -ic -c -t 200
```

## Vhosts fuzzing

```bash
ffuf -w <wordlist> -u https://target.com/ -H 'Host: FUZZ.target.com' -ic -c -t 200
```

## Parameter and Value Fuzzing&#x20;

### GET Request

{% code overflow="wrap" %}
```bash
** Parameter fuzzing **
ffuf -w seclists/Discovery/Web-Content/burp-parameter-names.txt -u https://target.com/admin/admin.php?FUZZ=key -fs 900 -ic -c

** Value Fuzzing **
for i in $(seq 1 1000); do echo $i >> ids.txt; done
ffuf -w ./ids.txt -u https://target.com/admin/admin.php?id=FUZZ -fs 900 -ic -c
```
{% endcode %}

### POST Request

{% code overflow="wrap" %}
```bash
** Parameter Fuzzing **
ffuf -w <wordlist> -u https://target.com/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/X-www-form-urlencoded' -fs 900

** Value Fuzzing **
ffuf -w ./ids.txt -u https://target.com/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/X-www-form-urlencoded' -fs 900
```
{% endcode %}

### Fuzzing both parameter and value

{% code overflow="wrap" %}
```bash
** POST **
ffuf -w param_list.txt:PARAM -w value_list.txt:VAL -u https://target.com/admin/admin.php -X POST -d 'PARAM=VAL' -H 'Content-Type: application/X-www-form-urlencoded' -fs 900

** GET **
ffuf -w params.txt:PARAM -w values.txt:VAL -u https://target.com/?PARAM=VAL -mr "VAL" -c
#Fuzz multiple locations. Match only responses reflecting the value of "VAL" keyword. Colored
```
{% endcode %}

## Username Enumeration

### Narrow Difference

Response for valid username: `Invalid username or password`

Response for invalid username: `Invalid username or password.`

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

### Via response timing



### Most Probably working irrespective of difference

> Works only if you have correct set of both username and password in the list.&#x20;

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

### FFUF Returns 403, But Same Request via Burp Gives 200 – Why?

When sending FFUF requests **directly**, the server may respond with `403 Forbidden` due to one or more of the following reasons:

* **Strict WAF or server rules** blocking non-browser-like behavior.
* **Missing TLS fingerprinting**: Tools like FFUF don’t mimic real browser TLS handshakes or JA3 fingerprints.
* **Burp Suite adds browser-like behavior** (e.g., better TLS handshake, redirects, header normalization), which can bypass simple WAFs.
* Some servers inspect **TLS client hello**, **order of headers**, or subtle fingerprinting details to allow/block requests.

✅ **Result**:\
**Burp (proxying)** acts more like a browser → gets `200 OK`\
**FFUF (direct)** acts like a scanner → gets `403 Forbidden`

{% code overflow="wrap" %}
```bash
ffuf -request req.txt -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -c -x http://127.0.0.1:8080 -t 100 -ic -c
```
{% endcode %}

## ffufai

ffuf + AI ⇒ ffufai

{% hint style="info" %}
It requires OpenAI Premium.&#x20;
{% endhint %}

**Installation script \[with dependencies installed in venv]**

```bash
#!/bin/bash

# Variables
VENV_DIR="$HOME/.ffufai_venv"
INSTALL_DIR="/tmp/ffufai"
BIN_LINK="/usr/local/bin/ffufai"
LAUNCHER_PATH="$VENV_DIR/run_ffufai.sh"

# Step 1: Install required packages
echo "[*] Installing dependencies..."
sudo apt update
sudo apt install -y git python3 python3-venv

# Step 2: Clone the ffufai repository
echo "[*] Cloning ffufai..."
rm -rf "$INSTALL_DIR"
git clone https://github.com/jthack/ffufai.git "$INSTALL_DIR"

# Step 3: Create virtual environment
echo "[*] Creating virtual environment at $VENV_DIR..."
python3 -m venv "$VENV_DIR"

# Step 4: Install Python dependencies inside venv
echo "[*] Installing Python dependencies..."
source "$VENV_DIR/bin/activate"
pip install --upgrade pip
pip install requests openai anthropic
deactivate

# Step 5: Copy ffufai.py to the venv
cp "$INSTALL_DIR/ffufai.py" "$VENV_DIR/"

# Step 6: Create launcher script
echo "[*] Creating launcher script..."
cat <<EOF > "$LAUNCHER_PATH"
#!/bin/bash
source "$VENV_DIR/bin/activate"
python "$VENV_DIR/ffufai.py" "\$@"
deactivate
EOF

chmod +x "$LAUNCHER_PATH"

# Step 7: Create global symlink
echo "[*] Linking launcher globally as 'ffufai'..."
sudo ln -sf "$LAUNCHER_PATH" "$BIN_LINK"

# Step 8: Ask for OpenAI API key
read -p "Enter your OpenAI API key: " openai_key

# Step 9: Add key to ~/.zshrc (only if not present)
if ! grep -q "OPENAI_API_KEY=" "$HOME/.zshrc"; then
    echo "export OPENAI_API_KEY=\"$openai_key\"" >> "$HOME/.zshrc"
    echo "[+] API key added to ~/.zshrc"
else
    echo "[!] OPENAI_API_KEY already exists in ~/.zshrc"
fi

echo "[✔] ffufai is now globally available!"
echo "[👉] Just run: ffufai <arguments>"
```
