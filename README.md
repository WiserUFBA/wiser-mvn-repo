#wiser-mvn-repo

Wiser Maven Repository

## Como adicionar um projeto  a este repositorio 

A primeira coisa a se fazer é incluir isso no pom do seu projeto

```xml
<distributionManagement>
    <repository>
        <id>release-repo</id>
        <url>https://github.com/WiserUFBA/wiser-mvn-repo/raw/master/releases</url>
    </repository>
    <snapshotRepository>
        <id>snapshot-repo</id>
        <url>https://github.com/WiserUFBA/wiser-mvn-repo/raw/master/snapshots</url>
    </snapshotRepository>
</distributionManagement>
```

As diretivas acima configuram questões relacionadas a como irá ocorrer a distribuição
do artefatos do maven. 

Feito isso utilize este comando no terminal para fazer o deploy local substituindo 
a `<url>` com a pasta local do clone deste repositorio, é importante que você tenha
clonado este repositório em seu computador.

`mvn -DaltDeploymentRepository=<reponame>::default::file:<url> deploy`

Como as modificações foram feitas localmente, você irá precisar dar commit e push
consequentemente para que as modificações sejam submetidas.

## Como incluir nossos repositórios em seu projeto

Como este repositório funciona como um servidor do maven tudo que você precisa é incluir
essa diretiva no pom do seu projeto para ter acesso a todos nossos repositórios
escolhendo qual versão você deseja ou seja **SNAPSHOT** ou **RELEASES**

### Snapshot

```xml
<repositories>
    <repository>
        <id>wiser-snapshots</id>
        <url>https://github.com/WiserUFBA/wiser-mvn-repo/raw/master/snapshots</url>
    </repository>
</repositories>
```

### Releases

```xml
<repositories>
    <repository>
        <id>wiser-releases</id>
        <url>https://github.com/WiserUFBA/wiser-mvn-repo/raw/master/releases</url>
    </repository>
</repositories>
```

### Pacotes Disponiveis

#### DriverMQTT

##### V-1.0

```xml
<dependency>
    <groupId>br.ufba.dcc.wiser</groupId>
    <artifactId>drivermqtt</artifactId>
    <version>1.0</version>
</dependency>
```

##### V-1.1

```xml
<dependency>
    <groupId>br.ufba.dcc.wiser</groupId>
    <artifactId>drivermqtt</artifactId>
    <version>1.1</version>
</dependency>
```

## Observações sobre compilação de bundles

Para compilar os bundles é preciso fazer duas coisas que são incluir este plugin
ao POM do seu projeto que será o responsavel por empacotar todas as dependências
num mesmo jar que irá ter a seguinte forma {nome}-jar-with-dependencies.jar (ou
algo próximo disso):

```xml
<plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <version>2.2.1</version>
        <configuration>
        <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
    </configuration>
    <executions>
        <execution>
            <id>make-assembly</id> 
            <phase>package</phase>
            <goals>
                <goal>single</goal>
            </goals>
        </execution>
    </executions>
</plugin>

```

Após esse passo concluido temos que ir nas nossas dependências e falar que todas
as dependências já presentes no ServiceMix não precisam ser empacotadas. Para isso adicione está linha `<scope>provided</scope>` a estas dependências lembrando
é claro que você deve excluir os Drivers deste processo já que eles precisam ter
suas dependências dentro do pacote. A forma final de cada dependência deve ser 
algo assim:

```xml
...
<dependency>
    <groupId>org.apache.servicemix.specs</groupId>
    <artifactId>org.apache.servicemix.specs.jsr339-api-2.0</artifactId>
    <scope>provided</scope>
</dependency>
...
```

--
<p align="center">
	Developed by </br>
  <img src="https://wiki.dcc.ufba.br/pub/SmartUFBA/ProjectLogo/wiserufbalogo.jpg"/>
</p>

