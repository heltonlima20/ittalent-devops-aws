# ittalent-devops-aws
Práticas para AWS - Curso IT Talent

# Criação Bucket S3

- Faça a criação de um bucket s3 na região sa-east-1 e desmarque a opção de bloquear todo o acesso público.
- Realize Upload no bucket dos arquivos estáticos do site (index.html - index.css - index.js)

- index.html
```
<!DOCTYPE html>
<html>
	<head>
		<title>S3 Static Site</title>
		<link rel='stylesheet' href='index.css'>
		<script src='index.js'></script>
	</head>
	<body>
		<h1>Hello world!</h1>
		<button onclick='onClick()'>Click me!</button>
	</body>
</html>
```

- index.css
```
body {
    background-color: #002130;
    color: #f7f7f7;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
```

- index.js
```
function onClick() {
    alert('Click!');
}
```

- Na página do painel do seu bucket, clique na guia Propriedades .
- Role para baixo até Hospedagem de site estático e clique em Editar .
- Selecione Ativar e depois “Hospedar um site estático”. Escreva index.html ou a página de índice do seu projeto existente como documento de índice.
- Na seção Política de bucket clique em Editar e cole o seguinte JSON, substituindo <BUCKET_NAME> pelo nome que você escolheu para seu bucket S3:

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Principal": "*",
			"Effect": "Allow",
			"Action": [
				"s3:GetObject"
			],
			"Resource": [
				"arn:aws:s3:::<BUCKET_NAME>/*"
			]
		}
	]
}
```

# Criação Instancia EC2

- Realizar criação de duas instancias EC2 na mesma região (sa-east-1)
- crie um grupo de segurança e adicione as duas instancias no mesmo grupo
- modifique as regras de roteamento do grupo de segurança e permita o tráfego ICMP para todos os destinos
- teste conectividade via ping entre as duas EC2
- instale o Nginx em ambas as EC2
- Com o Nginx instalado modifique a pagina default do nginx e faça um proxy-pass para o bucket que você criou
```
server {
    listen 80;

    location / {
        proxy_pass ENDEREÇO-DO-BUCKET/;
    }
}

```


# Criação de Load Balancer
- Modo aplicação
- criar grupo de destino e realizar o apontamento para modalidade de instancia
- Selecionar as duas instancias como ponto de destino
- utizar o protocolo http 80
- Depois de criado o grupo de destino registrar as duas instancias como ponto de acesso
- Finalizar a criação do load balancer
- Utilizar DNS do load balancer para testar comunicação
