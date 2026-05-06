# LabFoto-AWS-S4

## Los Requisitos Necesarios
```bash
terraform 
aws
node
```

## 1. La Configurar AWS
```bash
aws configure
# Access Key ID, Secret, Region: us-east-1
```

## 2. Clonar e Instalar
```bash
git clone https://github.com/Angeltr1248/AWS-Lambda-Integration_TorresRuizAngelEduardo.git && cd labfoto
cd src/upload-lambda && npm install && cd ../..
cd src/crop-lambda && npm install && cd ../..
```

## 3. La Crearción de terraform.tfvars
```hcl
environment   = "dev"
aws_region    = "us-east-1"
bucket_suffix = "tu-nombre-unico"
```

## 4. La Empaquetación de Lambdas
```bash
cd src/upload-lambda && zip -r ../upload-lambda.zip . && cd ../..
cd src/crop-lambda && zip -r ../crop-lambda.zip . && cd ../..
```

## 5. El Desplegar
```bash
cd terraform && terraform init
terraform apply -var-file=../terraform.tfvars
```

## 6. El Obtener API URL
```bash
lo que nos arroja un 
terraform output api_url
```

## 7. Probamos
```bash
curl -X POST https://tu-api-url/upload \
  -H "Content-Type: image/png" --data-binary @imagen.png
```

## 8. Ver Logs
```bash
aws logs tail /aws/lambda/upload-lambda-dev --follow
aws logs tail /aws/lambda/crop-lambda-dev --follow
```

## 9. Finalemente Destruir
```bash
cd terraform && terraform destroy -var-file=../terraform.tfvars
```

## 10. El Flujo
```
Cliente -> API Gateway -> Lambda Upload -> S3/uploads/->SQS Queue->Lambda Crop -> S3(bucket)/processed/
```
## 11. URLs Importantes
- API: `https://xxxxx.execute-api.us-east-1.amazonaws.com/upload`
- S3 Original: `s3://image-processor-dev-images-SUFIJO/uploads/`
- S3 Procesada: `s3://image-processor-dev-images-SUFIJO/processed/`

## Diferentes comandos
```bash
terraform plan -var-file=../terraform.tfvars    # Ver cambios
terraform show                                   # Ver estado
terraform state list                            # Recursos creados
aws s3 ls s3://image-processor-dev-images-*/uploads/  # Ver imágenes
```