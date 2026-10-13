# SUBMERSE S3 access starter

This folder contains a minimal Jupyter notebook for authorised, read-only access to SUBMERSE DAS HDF5 recordings through the MinIO S3 API.

1. Create a Python environment and install dependencies: `python -m pip install -r requirements.txt`.
2. Start Jupyter and open `SUBMERSE_S3_Access_Starter.ipynb`.
3. Supply your individual S3 credentials when prompted, or via `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables.
4. Set an accessible object prefix and select a recording.
5. Inspect metadata and read a bounded subset with readahead buffering.

The notebook intentionally does not implement analysis or visualisation. Never commit credentials to version control. Certificate verification is enabled. For systems with a custom CA bundle, set `SSL_CERT_FILE` to the appropriate path.

The tested environment used an older `s3fs` release; dependency ranges here are intended for new installations and have not been independently integration-tested against the MinIO service. If your environment has compatibility issues, use a tested environment-specific lockfile.
