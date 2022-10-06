# AWS CLI

O comando abaixo é utilizado para configurar a CLI da AWS:

```bash
aws configure
```

Após digita-lo basta inserir as informações da credencial que serão solicitadas.
As informações serão armazenadas na pasta `.aws` que será criada na raiz do usuário que executar o comando.

## AWS CLI em instâncias EC2

- NUNCA digite suas credenciais dentro de uma instância EC2 pois ficarão expostas a todas as pessoas que possuírem acesso a instância.
- IAM Role DEVE ser anexado ao EC2.
- IAM Role pode ter um policy que diz exatamente o que a EC2 pode fazer.
- EC2 pode ser anexada somente a um IAM Role mas a Role pode ser anexada a várias instâncias EC2.
- EC2 pode usar IAM sem nenhuma configuração adicional.
- A MELHOR pratica é utilizar EC2 com IAM Roles.

## AWS CLI - Dry Run

- Usamos o Dry Run para testar se temos permissão para executar comandos.
- O comando não será realmente executado.
- Somente alguns comando CLI possuem a flag `--dry-run` disponível para simular chamadas a API.
- Exemplo de comando com a flag `--dry-run`:

```bash
aws ec2 run-instances --dry-run --image-id ami-0895310529c333a0c --instance-type t2.micro
```

O retorno será algo como:

> An error occurred (DryRunOperation) when calling the RunInstances operation: Request would have succeeded, but DryRun flag is set.

## AWS CLI - STS Decode Errors

- Quando você executa uma chamada a API, você pode receber uma grande mensagem com erro e esse erro pode ser decodificado usando o STS.

Abaixo o comando para decodificar as mensagens de erro retornadas pela CLI ao executar um comando:

```bash
aws sts decode-authorization-message --encoded-message 0F74poFcBlzB3BxhjeB6JEW3c6xXu14Ab-FQOH4_pzuqtlWPMdAq50WCMbnMMnF0WaqSFd430xsMyM__ZNSA4pUhm-tt7unlYJ1-AcLTDkj9JuRRQvTcnUDLY_CCPpBBlNEC0bF3qwFjyn7An-F24O53LV-whRLHECWAmMbWt_iqIf6iM3YvtKpGdImypmQ6mMpRiiTNgvble_UhVVdE36DP35DOydQDaTHlFf9W32--YTD_tUgypYXfi7jyoJoopGu_LGcG1xIkBQfl9xWazSqFPM5M2wNUaBC7a-QqtuTnWI0wpq8Bj3toBPwOo8-R2gmBN655IdKhKAmkXRSZqs1PnUigrPqHJE1h4w6Tp9BQ0dEMfWhXDQCe6K1G0k6dr90ymzgNX9dK-HxFNXKkzwd-NKpk_JJd3zugcG6-RpvZ6cpbjD39WqI7zQtgif6-FWPE9Vj9QTR2aRR-XoV-TgjQqILh_VYSL_p-ueiMZkLzGS8ZapkU8ftUrovD5RjyneFtxjvyo8VgEQUyQSPHKMpRsWBvjMDGJS4NfehvLC3Jxyrby7DxSuNdYghegQHQZS3xBm8RieUrF9dvI8xzFwtf8htmybm5zh5XFtg_bd4oSpBxTcGoO2wyBdryOnDjAadqwOy_s6eOb3-GpSRendOPmTgLT0BvqOI9blxNrKKrECzb9hSkEIV6Ir0
```

O retorno será algo como:

```bash
{
    "DecodedMessage": "{\"allowed\":false,\"explicitDeny\":false,\"matchedStatements\":{\"items\":[]},\"failures\":{\"items\":[]},\"context\":{\"principal\":{\"id\":\"AROAXTXDXL2LZQ3QDGX2S:i-0095d64bf2f1f4d57\",\"arn\":\"arn:aws:sts::123456789:assumed-role/EC2ParaS3/i-0095d64bf2f1f4d57\"},\"action\":\"ec2:RunInstances\",\"resource\":\"arn:aws:ec2:sa-east-1:523389853335:instance/*\",\"conditions\":{\"items\":[{\"key\":\"ec2:InstanceMarketType\",\"values\":{\"items\":[{\"value\":\"on-demand\"}]}},{\"key\":\"aws:Resource\",\"values\":{\"items\":[{\"value\":\"instance/*\"}]}},{\"key\":\"aws:Account\",\"values\":{\"items\":[{\"value\":\"523389853335\"}]}},{\"key\":\"ec2:AvailabilityZone\",\"values\":{\"items\":[{\"value\":\"sa-east-1a\"}]}},{\"key\":\"ec2:ebsOptimized\",\"values\":{\"items\":[{\"value\":\"false\"}]}},{\"key\":\"ec2:IsLaunchTemplateResource\",\"values\":{\"items\":[{\"value\":\"false\"}]}},{\"key\":\"ec2:InstanceType\",\"values\":{\"items\":[{\"value\":\"t2.micro\"}]}},{\"key\":\"ec2:RootDeviceType\",\"values\":{\"items\":[{\"value\":\"ebs\"}]}},{\"key\":\"aws:Region\",\"values\":{\"items\":[{\"value\":\"sa-east-1\"}]}},{\"key\":\"aws:Service\",\"values\":{\"items\":[{\"value\":\"ec2\"}]}},{\"key\":\"ec2:InstanceID\",\"values\":{\"items\":[{\"value\":\"*\"}]}},{\"key\":\"aws:Type\",\"values\":{\"items\":[{\"value\":\"instance\"}]}},{\"key\":\"ec2:Tenancy\",\"values\":{\"items\":[{\"value\":\"default\"}]}},{\"key\":\"ec2:Region\",\"values\":{\"items\":[{\"value\":\"sa-east-1\"}]}},{\"key\":\"aws:ARN\",\"values\":{\"items\":[{\"value\":\"arn:aws:ec2:sa-east-1:523389853335:instance/*\"}]}}]}}}"
}
```

Com isso podemos formatar o JSON retornado e pronto, teremos a mensagem decodificada.
