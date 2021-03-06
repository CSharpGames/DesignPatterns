## Abstract Factory

Fornecer uma interface para criação de famílias de objetos relacionados ou dependentes sem especificar suas classes concretas

Encapsula a escolha da ckasse cibcreta a ser utilizada na criação de objetos de diversas familias.

***
#### Diagrama de classe
***

![abstractfactory](https://cloud.githubusercontent.com/assets/14116020/26184271/3f08e3ec-3b5a-11e7-9113-02fff283e053.png)

* **FabricaAbstrata (FabricaDeCarro)**: Interface que define as assinaturas dos métodos responsáveis pela criação dos objetos uma família.

* **FabricaConcreta (FabricaFiat, FabricaFord, ...)**: Classe que implementa os métodos de criação dos objetos de uma família.

* **ProdutoAbstrato (CarroSedan, CarroPopular)**: Interface que define um tipo de produto ou família de produtos.

* **ProdutoConcreto (Siena, FiestaSedan, Palio, Fiesta)**: Implementação particular dessas informações um tipo/família de produto.

* **Cliente**: Usa apenas as interfaces FabricaAbstrata e ProdutoAbstrato.

***
#### Implementação
***

1. Defina as interfaces que irão definir as familias de produtos (**ProdutoAbstrato**)

    ```c#
    namespace ProdutosAbstratos {
      public interface CarroPopular {
        void exibirInformacao();
      }
    
      public interface CarroSedan {
        void exibirInformacao();
      }
    }
    ```

2. Define os produtos de cada tipo/família (**ProdutoConcreto**)

    ```c#
    namespace ProdutosConcretos {
      public class Palio: CarroPopular {
        public void exibirInformacao() {
          Console.WriteLine("Modelo: Palio");
          Console.WriteLine("Fábrica: Fiat");
          Console.WriteLine("Categoria: Popular");
        }
      }
    
      public class Siena: CarroSedan {
        public void exibirInformacao() {
          Console.WriteLine("Modelo: Siena");
          Console.WriteLine("Fábrica: Fiat");
          Console.WriteLine("Categoria: Sedan");
        }
      }
    
      public class Fiesta: CarroPopular {
        public void exibirInformacao() {
          Console.WriteLine("Modelo: Fiesta");
          Console.WriteLine("Fábrica: Ford");
          Console.WriteLine("Categoria: Popular");
        }
      }
    
      public class FiestaSedan: CarroSedan {
        public void exibirInformacao() {
          Console.WriteLine("Modelo: Fiesta");
          Console.WriteLine("Fábrica: Ford");
          Console.WriteLine("Categoria: Sedan");
        }
      }
    }
    ```

3. Defina a fabrica que ira definir as fabricas concretas (**FabricaAbstrata**) passando os tipos de produtos que ela irá fabricar

    ```c#
    namespace FabricaAbstrata {
      public interface FabricaDeCarro {
        CarroSedan criarCarroSedan();
        CarroPopular criarCarroPopular();
      }
    }
    ```

4. Defina as fabricas concretas que irão criar os carros (**FabricaConcreta**)

    ```c#
    namespace FabricasConcretas {
      public class FabricaFiat: FabricaDeCarro {
        public CarroSedan criarCarroSedan() {
          return new Siena();
        }
    
        public CarroPopular criarCarroPopular() {
          return new Palio();
        }
      }
    
      public class FabricaFord: FabricaDeCarro {
        public CarroSedan criarCarroSedan() {
          return new FiestaSedan();
        }
    
        public CarroPopular criarCarroPopular() {
          return new Fiesta();
        }
      }
    }
    ```

5. Crie o cliente que usará dessas fabricas e carros

    ```c#
    class Teste {
      public static void Main(string[] args) {
        FabricaDeCarro fabrica = new FabricaFiat();
        CarroSedan sedan = fabrica.criarCarroSedan();
        CarroPopular popular = fabrica.criarCarroPopular();
        sedan.exibirInformacao();
        popular.exibirInformacao();
    
        fabrica = new FabricaFord();
        sedan = fabrica.criarCarroSedan();
        popular = fabrica.criarCarroPopular();
        sedan.exibirInformacao();
        popular.exibirInformacao();
      }
    }
    ``` 
