# Lab 52: Network File Transfers (wget/curl)

## 1. What is Network File Transfer?

Network File Transfer means sending or downloading files between computers over a network.

In Linux, we can use tools like `wget` and `curl` to download files and webpages.

## 2. Lab Objectives

- Learn how to use `wget`.
- Learn how to use `curl`.
- Download files from a web server.
- Download and save webpages.
- Resume interrupted downloads.
- Verify downloaded files using checksums.

## 3. Important Tools

### wget

`wget` is a Linux command-line tool used to download files from the internet or a web server.

Example:

```bash
wget http://localhost:8000/sample-file.txt
```

### curl

`curl` is a command-line tool used to transfer data from or to a server.

Example:

```bash
curl http://localhost:8000/index.html
```

### Python HTTP Server

Python can create a simple local web server for testing file downloads.

Command:

```bash
python3 -m http.server 8000 &
```

Port `8000` is used by the local HTTP server.

## 4. Install Required Packages

```bash
sudo apt update
sudo apt install -y wget curl python3
```

Check versions:

```bash
wget --version | head -n 1
curl --version | head -n 1
python3 --version
```

## 5. Create the Lab Files

Create directories:

```bash
mkdir -p ~/lab52/served ~/lab52/downloads
cd ~/lab52/served
```

Create a sample text file:

```bash
echo "Hello from the lab server" > sample-file.txt
```

Create an HTML webpage:

```bash
cat > index.html <<'EOF'
<!DOCTYPE html>
<html>
<head><title>Lab 52</title></head>
<body><h1>Network File Transfers</h1></body>
</html>
EOF
```

Create a 50 MB test file:

```bash
dd if=/dev/urandom of=big.bin bs=1M count=50
```

Start the server:

```bash
python3 -m http.server 8000 &
```

## 6. Task 1: Download a File Using wget

Move to the downloads directory:

```bash
cd ~/lab52/downloads
```

Download the sample file:

```bash
wget 'http://localhost:8000/sample-file.txt'
```

Verify the downloaded file:

```bash
ls -l sample-file.txt
cat sample-file.txt
wc -c sample-file.txt
```

**Result:**
- Download successful.
- HTTP status: `200 OK`
- File size: 26 bytes.
- Content: `Hello from the lab server`

## 7. Task 2: Download a Webpage Using curl

Display the webpage:

```bash
curl --noproxy '*' 'http://localhost:8000/index.html'
```

Save webpage content into a file:

```bash
curl --noproxy '*' -s 'http://localhost:8000/index.html' -o webpage.html
```

View the downloaded webpage:

```bash
head -n 5 webpage.html
```

Compare original and downloaded files:

```bash
diff -u ~/lab52/served/index.html webpage.html
```

Check file size:

```bash
wc -c webpage.html
```

**Result:**
- HTTP status: `200 OK`
- Webpage downloaded successfully.
- File size: 111 bytes.
- No differences between original and downloaded HTML.

**Note:** `--noproxy '*'` bypasses the configured proxy for local requests.

## 8. Task 3: Resume an Interrupted Download

A partial download happens when a file download stops before completion.

Create a partial download:

```bash
timeout -s INT 2 wget --no-proxy --limit-rate=1m 'http://localhost:8000/big.bin'
```

Check the partial file:

```bash
ls -lh big.bin
wc -c big.bin
```

**Verified partial size:** 2,294,731 bytes.

### Resume the Download

The `-c` option means continue. It allows Wget to resume an existing partial download.

```bash
wget --no-proxy -c 'http://localhost:8000/big.bin'
```

Check the final file size:

```bash
wc -c big.bin
```

**Result:**
- Download resumed successfully.
- Final size: 52,428,800 bytes (50 MB).

## 9. Verify File Integrity Using SHA-256

A checksum helps verify that a downloaded file is identical to the original file.

Command:

```bash
sha256sum big.bin ~/lab52/served/big.bin
```

**Verified SHA-256 checksum:**

`af3ac02df2cf0efb36c43c4ecb558e7180ac912cba3fad013c2a553821ca3aba`

Both files produced the same checksum.

**Result:** File integrity verified successfully.

## 10. Important Commands Revision

| Command | Purpose |
|---|---|
| `wget URL` | Download a file |
| `curl URL` | Retrieve data from a server |
| `curl -o file URL` | Save downloaded content |
| `wget -c URL` | Resume a partial download |
| `python3 -m http.server 8000` | Start a local HTTP server |
| `wc -c file` | Check file size in bytes |
| `sha256sum file` | Calculate file checksum |
| `diff file1 file2` | Compare two files |

## 11. Lab Summary

In this lab, I learned how to use `wget` and `curl` for network file transfers.

I successfully:
- Installed and verified the required tools.
- Created a local HTTP server.
- Downloaded a text file using Wget.
- Retrieved and saved an HTML webpage using cURL.
- Created and resumed an interrupted 50 MB download.
- Verified file integrity using SHA-256 checksums.

**Final Result:** All three practical tasks were successfully completed and verified.
