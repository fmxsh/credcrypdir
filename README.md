# credcrypdir - Encrypt and Decrypt Directories

Is a wrapper uniting the `crypdir` and `cred` commands to encrypt and decrypt directories.

Gets encryption keys and PBKDF2 iteration count from a credentials file created by the `cred` command. Then encrypts or decrypts a directory using the retrieved keys and iteration count.

## Features

- **Encrypt (-e):** Compress and encrypt a directory into a secure file.
- **Decrypt (-d):** Decrypt and decompress an encrypted file into a directory.
- Retrieves encryption keys and PBKDF2 iteration count using a credential file (`cred`).
- Password input can be provided interactively or piped.

## Requirements

- `crypdir`: Tool for encryption (`-e`) and decryption (`-d`).
- `cred`: Used for retrieving encryption keys and iteration counts.

> **Important:**  
> When specifying `<cred keys>`, they must include `KEY` and `ITER` in that order like "settings_key settings_iter".

## Usage

### Command Syntax

```bash
./credcrypdir [-d|-e] <credfile> <cred keys> <source> <target>
```

### Options

> [!NOTE]
> The directory itself is included, not only its contents. Encrypting with <source> set to ./my-dir will decrypt to ./my-dir.
> The whole path is included, so encrypting with <source> set to ./a/b/c will decrypt to ./a/b/c. Any other content of a and b is not included, only content of c.

- `-d`: Decrypt and decompress a file.
- `-e`: Encrypt and compress a directory.
- `<credfile>`: Credential file for retrieving encryption keys and iteration counts.
- `<cred keys>`: These are the names of the keys (space separated) in the credentials vault holding encryption key and iteration count (e.g., `MY_KEY MY_ITER`).
- `<source>`:
  - For `-d`: Encrypted file to decrypt.
  - For `-e`: Directory to encrypt and compress.
- `<target>`:
  - For `-d`: Directory where decrypted files will be extracted.
  - For `-e`: Output file name for the encrypted archive.

## Examples

### Encrypt a Directory

```bash
./script.sh -e credfile "KEY ITER" my_directory output_file.enc
```

- Encrypts and compresses `my_directory` into `output_file.enc`.
- Retrieves the encryption key and iteration count from `credfile` using the `cred` command.

### Decrypt a File

```bash
./script.sh -d credfile "KEY ITER" encrypted_file.enc target_directory
```

- Decrypts `encrypted_file.enc` and extracts its contents into `target_directory`.
- Retrieves the decryption key and iteration count from `credfile`.

## Password Handling

- If data is piped into the script, the password is read from the pipe.
- If not, the script prompts for the password interactively.

## Security Notes

- Use strong and unique passwords for each operation.
- Store the credential file (`credfile`) securely to prevent unauthorized access.
- Consider increasing the PBKDF2 iteration count for added security.

## Error Handling

- The script ensures all necessary inputs (`KEY` and `ITER`) are retrieved successfully from the `cred` command.
- Provides clear error messages for any issues during encryption or decryption.
