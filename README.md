# IoC-DI

Aqui estão exemplos de **Inversão de Controle (IoC)** e **Injeção de Dependência (DI)** utilizando interfaces, que ajudam a reduzir o acoplamento entre as classes.

### 1. Exemplo de **Inversão de Controle (IoC)** com Interface

Neste exemplo, o controle da criação das dependências é invertido e gerenciado externamente (fora da classe `Service`).

```java
// Interface para a dependência
public interface Repository {
    void save();
}

// Implementação da interface
public class MySQLRepository implements Repository {
    @Override
    public void save() {
        System.out.println("Saving to MySQL database");
    }
}

// Outra implementação da interface
public class PostgreSQLRepository implements Repository {
    @Override
    public void save() {
        System.out.println("Saving to PostgreSQL database");
    }
}

// Classe que usa a interface, sem saber qual implementação está sendo usada
public class Service {
    private Repository repository;

    // Inversão de controle: a dependência é injetada externamente
    public Service(Repository repository) {
        this.repository = repository;
    }

    public void doSomething() {
        repository.save(); // A implementação de Repository será chamada
    }
}
```

**Uso da Inversão de Controle**:
```java
public class App {
    public static void main(String[] args) {
        // A criação da dependência é feita fora do Service (IoC)
        Repository repo = new MySQLRepository(); // Podemos trocar facilmente para PostgreSQLRepository
        Service service = new Service(repo);     // Dependência é injetada no Service

        service.doSomething(); // Chama o método, usando a implementação do Repository fornecida
    }
}
```

Neste caso, o framework ou o código externo (fora da `Service`) controla a injeção da dependência, o que caracteriza a Inversão de Controle.

---

### 2. Exemplo de **Injeção de Dependência (DI)** com Interface

Agora, vejamos o mesmo cenário, mas focando na **Injeção de Dependência**. A dependência é passada por meio do construtor, do método ou diretamente via campo.

#### Injeção por Construtor
```java
// Interface para a dependência
public interface Repository {
    void save();
}

// Implementação da interface
public class MySQLRepository implements Repository {
    @Override
    public void save() {
        System.out.println("Saving to MySQL database");
    }
}

// Classe Service que depende de Repository
public class Service {
    private Repository repository;

    // Injeção de dependência por construtor
    public Service(Repository repository) {
        this.repository = repository;
    }

    public void doSomething() {
        repository.save(); // Uso da dependência injetada
    }
}
```

**Uso da Injeção de Dependência**:
```java
public class App {
    public static void main(String[] args) {
        // Criação da dependência
        Repository repo = new MySQLRepository();

        // Injetando a dependência no construtor de Service
        Service service = new Service(repo);
        service.doSomething();
    }
}
```

#### Injeção por Setter (Método)
```java
public class Service {
    private Repository repository;

    // Injeção de dependência via método setter
    public void setRepository(Repository repository) {
        this.repository = repository;
    }

    public void doSomething() {
        repository.save();
    }
}

public class App {
    public static void main(String[] args) {
        Repository repo = new MySQLRepository();
        Service service = new Service();
        service.setRepository(repo);  // Injetando a dependência pelo método setter
        service.doSomething();
    }
}
```

### Diferença em Resumo
- **IoC**: A inversão do controle de criação e gerenciamento das dependências é feita por um container ou framework externo (como no exemplo com o `Service` recebendo a dependência).
- **DI**: Um tipo de IoC em que as dependências são fornecidas ao objeto através de construtores, setters ou campos, como visto acima.

Em ambos os exemplos, o `Service` depende de um `Repository`, mas a implementação específica (como `MySQLRepository` ou `PostgreSQLRepository`) pode ser trocada facilmente, o que torna o código mais flexível e testável.
