
# S3-and-RDS-setup


This guide provides instructions for setting up your backend application using PostgreSQL RDS, S3 for file storage, and Django framework.


## RDS Setup

### Create RDS Database
1. Navigate to the RDS console and create a database:
   - **Standard Create**
   - **Engine**: PostgreSQL
   - **Free Tier**
   - **Database Name**: `backenddemo`
   - **Master Username**: `mysuperuser`
   - **Master Password**: `mysuperuser`
   - **Allocated Storage**: 20 GB
   - **Public Access**: Yes
   - **Create Security Group**: `backend`

### Configure Django Settings
Update the `settings.py` file in your Django project to connect to the RDS database:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'dbdemo',
        'USER': 'mysuperuser',
        'PASSWORD': 'mysuperuser',
        'HOST': '<your-rds-endpoint>',
        'PORT': '5432',
    }
}
```

### Install Required Packages
Run the following commands to install dependencies:

```bash
pip install psycopg2-binary
pip install psycopg2
pip install storage
pip install django-storages
pip freeze > requirements.txt
```

### Configure Security Group
Add inbound rules in the security group to allow access to the database.

### Run Migrations and Create Superuser
Execute the following commands:

```bash
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

### Admin Configuration
1. Visit the admin panel in your browser.
2. Add categories and products.
3. Commit changes.

---

## S3 Setup

### Create S3 Bucket
1. Navigate to the S3 console and create a bucket:
   - Disable the **Block Public Access** option.
   - Acknowledge the warning.

### Update Django Settings for S3
Add the following to your `settings.py` file:

```python
AWS_ACCESS_KEY_ID = 'your_aws_access_key_id'
AWS_SECRET_ACCESS_KEY = 'your_aws_secret_access_key'
AWS_STORAGE_BUCKET_NAME = 'backenddemo'
AWS_S3_SIGNATURE_NAME = 's3v4'
AWS_S3_REGION_NAME = 'ap-south-1'
AWS_S3_FILE_OVERWRITE = False
AWS_DEFAULT_ACL = None
AWS_S3_VERIFY = True
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```

### Create IAM User
1. Create a new IAM user with **S3 Full Access** policy.
2. Generate an access key:
   - Select **Local Code**.
   - Check "I Understand".
   - Save the CSV file containing the access key.

### Install Required Packages
Run the following commands:

```bash
pip install boto3
pip install drf-yasg
```

### Test S3 Integration
1. Run the Django development server:

   ```bash
   python manage.py runserver
   ```

2. Open the admin panel in your browser.
3. Add a product and upload an image. The image will be automatically uploaded to S3.
4. Verify the uploaded image in the S3 bucket.

---

## Conclusion
By following these steps, you will have a fully functional backend setup with PostgreSQL RDS for your database and S3 for file storage. Ensure that you commit your changes and test thoroughly.



