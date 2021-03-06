## Visitor

Quando temos uma árvore, e precisamos navegar nessa árvore de maneira organizada, podemos usar um Visitor

Permitir atualizações específicas em uma coleção de objetos de acordo com o tipo parti- cular de cada objeto atualizado.

Representar uma operação a ser executada nos elementos de uma estrutura de objetos. Visitor permite definir uma nova operação sem mudar as
classes dos elementos sobre os quais opera.

***
#### Diagrama de classe
***

![visitor](https://cloud.githubusercontent.com/assets/14116020/26286200/b9bf9198-3e35-11e7-89b2-824fa3be45dd.png)

* **Visitante (AtualizadorDeFuncionario)**: Define a interface dos objetos responsáveis pelas atualizações dos Elementos.

* **VisitanteConcreto (AtualizadorSalarial)**: Implementa a lógica de uma determinada atualização dos Elementos.

* **Elemento (Atualizavel)**: Define a interface dos objetos que podem ser atualizados por um Visitante.

* **ElementoConcreto (Funcionario, Departamento)**: Define um tipo específico de Elemento.

* **EstruturaDoObjeto**: Agregador dos Elementos.

* **Cliente**: Aplica um determinado Visitante nos Elementos da estrutura.

***
#### Implementação
***

1. Defina a interface AtualizadorDeFuncionario (**Visitante**)

    ```c#
    namespace Visitante {
      public interface AtualizadorDeFuncionario {
        void atualiza(Gerente gerente);
        void atualiza(Telefonista telefonista);
      }
    }
    ```

2. Defina a implementação AtualizadorSalarial (**VisitanteConcreto**)

    ```c#
    namespace VisitantesConcretos {
      public class AtualizadorSalarial: AtualizadorDeFuncionario {
        public void atualiza(Gerente gerente) {
          gerente.setSalario(gerente.getSalario() * 1.43);
        }
    
        public void atualiza(Telefonista telefonista) {
          telefonista.setSalario(telefonista.getSalario() * 1.27);
        }
      }
    }
    ```

3. Crie a interface Atualizavel (**Elemento**).

    ```c#
    namespace Elemento {
      public interface Atualizavel {
        void aceita(AtualizadorDeFuncionario atualizador);
      }
    }
    ```

4. Defina as classes Funcionario, Gerente, Telefonista e Departamento (**ElementosConcretos**).

    ```c#
    namespace ElementosConcretos {
      public abstract class Funcionario: Atualizavel {
        private string nome;
        private double salario;
    
        public Funcionario(string nome, double salario) {
          this.nome = nome;
          this.salario = salario;
        }
    
        public string getNome() {
          return this.nome;
        }
    
        public double getSalario() {
          return this.salario;
        }
    
        public void setSalario(double salario) {
          this.salario = salario;
        }

        public abstract void aceita(AtualizadorDeFuncionario atualizador);
      }
    
      public class Gerente: Funcionario {
        private string senha;
    
        public Gerente(string nome, double salario, string senha): base(nome, salario) {
          this.senha = senha;
        }
    
        public string getSenha() {
          return this.senha;
        }
    
        public override void aceita(AtualizadorDeFuncionario atualizador) {
          atualizador.atualiza(this);
        }
      }
    
      public class Telefonista: Funcionario {
        private int ramal;
    
        public Telefonista(string nome, double salario, int ramal): base(nome, salario) {
          this.ramal = ramal;
        }
    
        public int getRamal() {
          return this.ramal;
        }
    
        public override void aceita(AtualizadorDeFuncionario atualizador) {
          atualizador.atualiza(this);
        }
      }
    
      public class Departamento {
        private string nome;
        private List<Funcionario> funcionarios = new List<Funcionario>();
    
        public Departamento(string nome) {
          this.nome = nome;
        }
    
        public string getNome() {
          return nome;
        }
    
        public List<Funcionario> getFuncionarios() {
          return funcionarios;
        }
    
        public void aceita(AtualizadorDeFuncionario atualizador) {
          foreach (Funcionario funcionario in this.funcionarios) {
            funcionario.aceita(atualizador);
          }
        }
      }
    }
    ```

5. Faça uma classe para testar o atualizador salarial.

    ```c#
    class Testes {
      public static void Main(string[] args) {
        List<Departamento> lista = new List<Departamento>();
        Departamento departamento1 = new Departamento("Departamento 1");
        Departamento departamento2 = new Departamento("Departamento 2");
        Funcionario gerente1 = new Gerente("Gerente 1", 1500, "1234");
        Funcionario gerente2 = new Gerente("Gerente 2", 1800, "1234");
        Funcionario telefonista1 = new Telefonista("Telefonista 1", 1000, 2);
        Funcionario telefonista2 = new Telefonista("Telefonista 2", 1200, 1);
    
        departamento1.getFuncionarios().Add(gerente1);
        departamento1.getFuncionarios().Add(telefonista1);
        departamento2.getFuncionarios().Add(gerente2);
        departamento2.getFuncionarios().Add(telefonista2);
    
        lista.Add(departamento1);
        lista.Add(departamento2);
    
        AtualizadorDeFuncionario atualizador = new AtualizadorSalarial();
    
        foreach (Departamento departamento in lista) {
          departamento.aceita(atualizador);
        }
    
        foreach (Departamento departamento in lista) {
          foreach (Funcionario funcionario in departamento.getFuncionarios()) {
            Console.WriteLine("Nome: " + funcionario.getNome());
            Console.WriteLine("Salário: " + funcionario.getSalario());
          }
        }
      }
    }
    ```
