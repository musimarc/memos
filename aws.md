# Copy deep archive glacier bucket to another one after restoring objects
aws s3 --profile <profile_name> mv s3://bucketName/sourceFolder s3://bucketName/destFolder --recursive --storage-class DEEP_ARCHIVE --force-glacier-transfer

# Copy all files from working directory to the big-datums bucket:
aws s3 cp ./ s3://big-datums/ --recursive

# Delete all ".java" files from the big-datums bucket:
aws s3 rm s3://big-datums/ --recursive --exclude "*" --include "*.java"

# Delete all files in the big-datums bucket with a file extension beginning 
# with "j" or "c" (".csv", ".java, ".json", ."jpeg", etc.):
aws s3 rm s3://big-datums/ --recursive --exclude "*" --include "*.[jc]*"

# Copy ".txt" and ".csv" files from big-datums S3 bucket to local working directory:
aws s3 cp s3://big-datums/ . --recursive --exclude "*" --include "*.txt" --include "*.csv"


