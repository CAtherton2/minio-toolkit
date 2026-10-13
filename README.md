# SUBMERSE S3 access starter

This folder contains a minimal Jupyter notebook for authorised, read-only access to SUBMERSE DAS HDF5 recordings through the MinIO S3 API.

1. Create a Python environment and install dependencies: `python -m pip install -r requirements.txt`.
2. Start Jupyter and open `SUBMERSE_S3_Access_Starter.ipynb`.
3. Supply your individual S3 credentials when prompted, or via `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables.
4. Set an accessible object prefix and select a recording.
5. Inspect metadata and read a bounded subset with readahead buffering.

The notebook intentionally does not implement analysis or visualisation. Never commit credentials to version control. Certificate verification is enabled. For systems with a custom CA bundle, set `SSL_CERT_FILE` to the appropriate path.

The tested environment used an older `s3fs` release; dependency ranges here are intended for new installations and have not been independently integration-tested against the MinIO service. If your environment has compatibility issues, use a tested environment-specific lockfile.

## Troubleshooting

### SSL certificate verification errors

When connecting to the SUBMERSE MinIO S3 endpoint, you may encounter:

```text
SSLCertVerificationError:
certificate verify failed:
self-signed certificate in certificate chain
```

This means Python cannot verify the certificate chain presented by the S3 endpoint using its currently configured trusted Certificate Authorities (CAs).

**Do not disable certificate verification using `verify=False`.**

#### 1. Check the certificate configuration

The notebook attempts to identify an available CA certificate bundle automatically.

To inspect the selected configuration:

```python
print("Selected CA bundle:", ca_bundle)
```

A file path indicates that a specific CA bundle is being used.  A value of `True` means that the Python library's default certificate verification configuration is being used.

#### 2. Specify a trusted CA bundle manually

If automatic detection fails, you can specify the location of a trusted CA certificate bundle using the `AWS_CA_BUNDLE` environment variable.

Common locations include:

| Operating system | Example |
|---|---|
| Ubuntu / Debian | `/etc/ssl/certs/ca-certificates.crt` |
| RHEL / Fedora | `/etc/pki/tls/certs/ca-bundle.crt` |
| macOS | Depends on the Python installation and certificate configuration |
| Windows | Depends on the Python installation and certificate configuration |

For example, on Linux or macOS:

```bash
export AWS_CA_BUNDLE="/path/to/trusted-ca-bundle.pem"
```

On Windows PowerShell:

```powershell
$env:AWS_CA_BUNDLE = "C:\path\to\trusted-ca-bundle.pem"
```

Restart the Jupyter kernel and rerun the connection cell after updating the environment.

The specified bundle must contain the CA certificates required to validate the S3 endpoint.  Merely selecting an existing certificate file does not guarantee that verification will succeed.

#### 3. Check other common errors

| Error | Possible cause |
|---|---|
| `AccessDenied` | The S3 account does not have permission for the requested operation |
| `InvalidAccessKeyId` or `SignatureDoesNotMatch` | Incorrect credentials or S3 client configuration |
| `EndpointConnectionError` | Network, DNS, firewall or endpoint connectivity problem |
| `SSLCertVerificationError` | Certificate chain cannot be validated against the configured trusted CAs |
| `NoSuchKey` | The requested recording path does not exist |

### Getting support

If a problem persists, provide the operating system, Python version, relevant error message and selected CA bundle path.

**Never share S3 access keys, secret keys or other authentication credentials when requesting support.**
