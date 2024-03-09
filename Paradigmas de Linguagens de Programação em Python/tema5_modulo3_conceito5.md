# Implementando Polimorfismo

Polimorfismo é o mecanismo que permite a um método com o mesmo nome ser executado de modo diferente a depender do objeto que está chamando o método. A linguagem define em tempo de execução (late binding) qual método deve ser chamado. Essa característica é bastante comum em herança de classes devido à redefinição da implementação dos métodos nas subclasses.

Vamos imaginar que agora tenhamos uma entidade Banco que controla todas as contas criadas no sistema de conta corrente. As contas podem ser do tipo `ContaCliente`, `ContaComum` ou `ContaRemunerada`.

O cálculo do rendimento da `ContaCliente` aumenta o valor investido com base na taxa de rendimento e, em seguida, desconta o IOF e o IR sobre o valor do rendimento. A `ContaComum`, que é uma subclasse de `ContaCliente`, sobrescreve o método CalculoRendimento para aumentar o valor investido com base na taxa de rendimento, mas não aplica nenhum desconto. Já a `ContaRemunerada`, outra subclasse de `ContaCliente`, sobrescreve o método CalculoRendimento para aumentar o valor investido com base na taxa de rendimento e, em seguida, desconta apenas o IOF sobre o valor do rendimento.

Todos os descontos são realizados em cima do valor bruto após o rendimento mensal. Uma vez por mês, o banco executa o cálculo do rendimento de todos os tipos de contas.

```python
import datetime

class ContaPoupanca:
   def __init__(self, taxaremuneracao):
      self.taxaremuneracao = taxaremuneracao
      self.data_abertura = datetime.datetime.today()
      self.saldo = 0

   def remuneracaoConta(self):
      self.saldo += self.saldo * self.taxaremuneracao


class ContaCliente:
  def __init__(self, numero, IOF, IR, valorinvestido, taxarendimento):
    self.numero = numero
    self.IOF = IOF
    self.IR = IR
    self.valorinvestido = valorinvestido
    self.taxarendimento = taxarendimento

  def CalculoRendimento(self):
    self.valorinvestido += (self.valorinvestido * self.taxarendimento)
    self.valorinvestido = self.valorinvestido - (self.taxarendimento * self.IOF * self.IR)

  def Extrato(self):
    print (f"O saldo atual da conta {self.numero} é {self.valorinvestido:10.2f}")


class ContaComum(ContaCliente):
    def __init__(self, numero, IOF, IR, valorinvestido, taxarendimento):
        super().__init__(numero, IOF, IR, valorinvestido, taxarendimento)

    def CalculoRendimento(self):
        self.valorinvestido += (self.valorinvestido * self.taxarendimento)


class ContaRemunerada(ContaCliente):
    def __init__(self, numero, IOF, IR, valorinvestido, taxarendimento):
        super().__init__(numero, IOF, IR, valorinvestido, taxarendimento)

    def CalculoRendimento(self):
        self.valorinvestido += (self.valorinvestido * self.taxarendimento)
        self.valorinvestido -= self.valorinvestido * self.IOF
    

class Banco():
    def __init__(self, codigo, nome):
        self.codigo = codigo
        self.nome = nome
        self.contas = []

    def adicionaconta(self, contacliente):
        self.contas.append(contacliente)

    def calcularendimentomensal(self):
        for c in self.contas:
            c.CalculoRendimento()

    def imprimesaldocontas(self):
        for c in self.contas:
            c.Extrato()


banco1 = Banco(999,"teste")
contacliente1 = ContaCliente(1, 0.01, 0.1, 1000, 0.05)
contacomum1 = ContaComum(2, 0.01, 0.1, 2000, 0.05)
`contaremunerada`1 = `ContaRemunerada`(3, 0.01, 0.1, 2000, 0.05)

banco1.adicionaconta(contacliente1) #(4)
banco1.adicionaconta(contacomum1) #(5)
banco1.adicionaconta(`contaremunerada`1) #(6)
banco1.calcularendimentomensal() #(7)
banco1.imprimesaldocontas() #(8)

```

Em (4), (5) e (6), o banco adiciona diferentes tipos de contas usando um único método. Esse método aceita qualquer objeto que seja uma `ContaCliente` ou qualquer coisa que seja um tipo de `ContaCliente` (como `ContaComum` e `ContaRemunerada`). Isso é possível graças a um recurso das linguagens de programação orientadas a objetos chamado ‘É-UM’.

Em (4), estamos adicionando uma `ContaCliente` ao banco. Em (5), estamos adicionando uma `ContaComum`, que é um tipo de `ContaCliente`. Em (6), estamos adicionando uma `ContaRemunerada`, que também é um tipo de `ContaCliente`. A linguagem de programação faz isso de forma transparente para o programador.

Em (7), vemos o polimorfismo em ação. O banco tem uma lista de contas de diferentes tipos. Quando o banco calcula o rendimento mensal, ele percorre cada conta na lista e chama o método CalculoRendimento(). Cada tipo de conta tem sua própria versão desse método, então mesmo que estejamos chamando o mesmo método em cada conta, o que realmente acontece depende do tipo de conta. Isso é o polimorfismo.

Em (8), chamamos o método Extrato() em cada conta. Este método não foi redefinido pelas subclasses `ContaComum` e `ContaRemunerada`, então elas usam a versão do método da superclasse `ContaCliente`.

O polimorfismo é muito útil quando você tem muitos tipos diferentes de uma classe e cada um tem sua própria maneira de fazer as coisas. Sem o polimorfismo, você teria que verificar o tipo de cada objeto e chamar o método correto manualmente. Com o polimorfismo, a linguagem de programação faz isso por você.

## Perguntas

 **O que é polimorfismo?**
 
 Polimorfismo é um conceito em programação orientada a objetos que permite que um método se comporte de maneiras diferentes dependendo do objeto que o está chamando. Isso é útil quando você tem várias classes que herdam de uma superclasse comum, mas precisam implementar um método de maneira diferente.
 
 **O que o método adicionaconta faz no código?**
 
 O método adicionaconta adiciona uma conta à lista de contas do banco. Ele aceita um objeto que é uma instância de `ContaCliente` ou de uma de suas subclasses.
 
 **O que acontece quando o método calcularendimentomensal é chamado?**
 
 Quando o método calcularendimentomensal é chamado, ele percorre todas as contas do banco e chama o método CalculoRendimento em cada uma delas. Isso atualiza o valor investido em cada conta com base em seu rendimento e descontos.
 
 **O que o método imprimesaldocontas faz?**
 
 O método imprimesaldocontas percorre todas as contas do banco e imprime o saldo de cada conta. Ele faz isso chamando o método Extrato em cada conta.
 
 **O que é o teste “É-UM” mencionado no texto?**
 
 O teste “É-UM” é uma maneira de verificar se um objeto é uma instância de uma determinada classe ou de uma de suas subclasses. No contexto do código, é usado para verificar se um objeto passado para o método adicionaconta é uma `ContaCliente` ou um tipo de `ContaCliente`.
 
 **O que acontece se eu tentar adicionar um objeto que não é uma `ContaCliente` ao banco?**
 
 Se você tentar adicionar um objeto que não é uma `ContaCliente` (ou uma de suas subclasses) ao banco, você não receberá um erro imediatamente. No entanto, você receberá um erro quando o banco tentar calcular o rendimento mensal, porque ele espera que cada conta tenha um método CalculoRendimento().
 
