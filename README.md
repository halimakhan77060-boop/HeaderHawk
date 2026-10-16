# WebWarden

**A lightweight, passive web security scanner that crawls a site and flags common misconfigurations.**

WebWarden maps a website, then checks security headers, cookie flags, information leaks, and risky forms, and produces a severity-ranked report. It is designed for learning, practice labs, and quick checks of sites you own.

> **Legal notice:** Only scan systems you own or have explicit written permission to test. Unauthorized scanning may be illegal. The authors accept no liability for misuse.

## Features

- Same-host breadth-first crawler with page limit, delay, and timeout controls
- Security header checks (CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy)
- Cookie flag checks (`Secure`, `HttpOnly`, `SameSite`)
- Information disclosure checks (`Server`, `X-Powered-By`, and similar headers)
- Form checks (passwords over HTTP or GET, POST forms with no obvious CSRF token)
- Console report sorted by severity, plus optional JSON output

## Requirements

- Python 3.8 or newer
- `pip`
- Git (to clone the repository)

## Installation

### Linux / macOS

```bash
git clone https://github.com/<your-username>/webwarden.git
cd webwarden

python3 -m venv venv
source venv/bin/activate

pip install -r requirements.txt
```

### Windows (PowerShell)

```powershell
git clone https://github.com/halimakhan77060-boop/HeaderHawk.git
cd webWarden

python -m venv venv
venv\Scripts\Activate.ps1

pip install -r requirements.txt
```

### Without a virtual environment

```bash
pip install requests beautifulsoup4
```

## Usage

```bash
python webscan.py http://localhost:3000 --i-have-permission
```

Save a JSON report and limit the crawl:

```bash
python webscan.py http://localhost:3000 --max-pages 30 --delay 0.5 --out report.json --i-have-permission
```

### Options

| Option | Description | Default |
|---|---|---|
| `url` | Start URL to scan | required |
| `--max-pages` | Maximum pages to crawl | `25` |
| `--delay` | Seconds between requests | `0.3` |
| `--timeout` | Request timeout in seconds | `10` |
| `--out` | Write a JSON report to this file | none |
| `--i-have-permission` | Confirm you are authorized to test the target | required |

## Safe practice targets

Run a deliberately vulnerable app locally (requires [Docker](https://docs.docker.com/get-docker/)):

```bash
# OWASP Juice Shop
docker run --rm -p 3000:3000 bkimminich/juice-shop

# DVWA
docker run --rm -p 8080:80 vulnerables/web-dvwa
```

Then scan `http://localhost:3000` or `http://localhost:8080`.

## Example output

```
[+] 200 http://localhost:3000/
Scan of http://localhost:3000 - 12 pages crawled, 7 findings

[MEDIUM] missing-header: Missing Content-Security-Policy
         at http://localhost:3000/
         fix: Add a Content-Security-Policy to reduce the impact of XSS.
```

## Roadmap

- [ ] Active checks (reflected input, SQL error messages, open redirects)
- [ ] TLS/certificate configuration checks
- [ ] HTML report output
- [ ] Authenticated scanning (session cookies)

## Contributing

Issues and pull requests are welcome. Please keep contributions focused on defensive, authorized-testing use cases.

## License

Released under the [MIT License](LICENSE).
