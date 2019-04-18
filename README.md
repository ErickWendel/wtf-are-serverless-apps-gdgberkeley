# WTF are serverless apps - GDGBerkely

Examples that I've used on this presentation
Slides: http://bit.ly/wtf-are-sls-apps-gdgberkeley

```shell

# gcloud builds submit --tag gcr.io/[PROJECT-ID]/helloworld
# gcloud projects create $PROJECT_NAME

POSTGRES_HOST=YOUR_DB_URL
POSTGRES_DB=d63ffjeh8p39dj
POSTGRES_SSL=true

----
PROJECT_NAME=wtfserverless
REPONAME=nodejs-with-postgres-api-example

git clone https://github.com/ErickWendel/$REPONAME.git
cd $REPONAME

gcloud config get-value project

gcloud config set project $PROJECT_NAME
IMAGE_NAME=gcr.io/$PROJECT_NAME/$REPONAME
echo "building image..."
gcloud builds submit --tag $IMAGE_NAME
echo "preparing to deploy..."

gcloud beta run deploy \
    --image $IMAGE_NAME \
    --set-env-vars="POSTGRES_HOST=$POSTGRES_HOST,POSTGRES_DB=$POSTGRES_DB,POSTGRES_SSL=$POSTGRES_SSL" \
    --region=us-central1 \
    --allow-unauthenticated \
    api-erickwendel


----

git clone https://github.com/ErickWendel/nodejs-with-postgres-api-example.git 
cd nodejs-with-postgres-api-example

gcloud config set project wtfserverless

gcloud builds submit --tag gcr.io/wtfserverless/nodejs-with-postgres-api-example

gcloud beta run deploy \
    --image gcr.io/wtfserverless/nodejs-with-postgres-api-example \
    --set-env-vars="POSTGRES_HOST=$POSTGRES_HOST,POSTGRES_DB=$POSTGRES_DB,POSTGRES_SSL=$POSTGRES_SSL" \
    --region=us-central1 \
    --allow-unauthenticated \
    api-erickwendel


---
echo "Testing..."
docker build -t erickwendel/nodejs-with-postgres-api-example .
 docker run -p 3000:3000 \
     -e "POSTGRES_HOST=$POSTGRES_HOST" \
     -e "POSTGRES_DB=$POSTGRES_DB" \
     -e "POSTGRES_SSL=$POSTGRES_SSL" \
     erickwendel/nodejs-with-postgres-api-example
```