<!--
Projeto de Época Especial de Linguagens de Programação II 2020/2021 (c) by Nuno
Fachada

Projeto de Época Especial de Linguagens de Programação II 2020/2021 is licensed
under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International
License.

You should have received a copy of the license along with this
work. If not, see <http://creativecommons.org/licenses/by-nc-sa/4.0/>.
-->

# Projeto de Época Especial de Linguagens de Programação II 2020/2021

## Introdução

Os grupos devem implementar o jogo **18 Ghosts** na forma de duas aplicações C#,
uma em consola, outra em Unity. O jogo deve ser PvP (_Player vs Player_), sem
qualquer tipo de inteligência artificial.

As regras do jogo estão explicadas
[neste ficheiro](https://secure.grupolusofona.pt/ulht/moodle/pluginfile.php/1011414/mod_assign/introattachment/0/18GHOSTS_EN_r1.pdf?forcedownload=1).
Para este projeto existem duas simplificações a estas regras:

* A primeira fase do jogo consiste em colocar os fantasmas no tabuleiro (no
  manual do jogo é a secção que diz "PLACING THE GHOSTS ON THE BOARD"). Em vez
  de colocação dos fantasmas um a um por parte dos jogadores, a vossa aplicação
  pode colocar os fantasmas automaticamente e aleatoriamente no tabuleiro, sem
  que a nota fique prejudicada.
* O jogo pode terminar assim que um jogador consiga retirar três fantasmas do
  castelo, independentemente da cor dos mesmos, tal como descrito no final do
  manual do jogo, na secção "VARIANT FOR SHORTER PLAY".

O jogo deve ser implementado em consola e em Unity. A lógica, regras e estado do
jogo (o chamado **modelo**), devem ser completamente independentes tanto da
interface de consola (`WriteLines`, `ReadLines`), como do Unity (ver Secção
[MVC pattern](#mvc-pattern)). Este modelo deve ser obrigatoriamente o mesmo em
ambas as implementações, devendo ser partilhado de uma forma que não implique
copiar os ficheiros de um lado para o outro. Existe forma de fazer isto ao nível
do Git usando [sub-módulos][submodules] (ver Secção [Sugestões para o uso de
sub-módulos em Git](#sugestões-para-o-uso-de-sub-módulos-em-git)).

Ambas as versões devem ser multi-plataforma, ou seja, devem funcionar em Linux
e macOS, pelo que devem ser evitados métodos e classes que apenas funcionam em
Windows.

Os grupos podem ter entre 1 a 3 elementos.

## Funcionamento da Aplicação

O funcionamento exato da aplicação é da responsabilidade de cada grupo. No
entanto, quando a aplicação começa, **deve ser claro como cada jogador joga**,
ou seja, o jogo deve ter instruções muito claras sobre que teclas fazem o quê.
Por outras palavras, os grupos devem ter em conta as regras importantes do
_game design_, pois serão tidas em conta na avaliação do projeto.

A aplicação deve funcionar em Windows, macOS e Linux. A melhor estratégia para
garantir que assim seja é testar o jogo em Linux (e.g., numa máquina virtual).
Algumas instruções incompatíveis com macOS e Linux são, por exemplo:

* [Console.Beep()](https://docs.microsoft.com/dotnet/api/system.console.beep)
* [Console.SetBufferSize()](https://docs.microsoft.com/dotnet/api/system.console.setbuffersize)
* [Console.SetWindowPosition()](https://docs.microsoft.com/dotnet/api/system.console.setwindowposition)
* [Console.SetWindowSize()](https://docs.microsoft.com/dotnet/api/system.console.setwindowsize)
* Entre outras.

As instruções que só funcionam em Windows têm a seguinte indicação na sua
documentação:

![The current operating system is not Windows.](img/notsupported.png "The current operating system is not Windows.")

## Considerações e sugestões técnicas

### MVC pattern

O [Model-View-Controller (MVC)][MVC] *design pattern* é o ponto de partida para
a realização deste projeto. Alguns Época Especials que podem utilizar para compreender
como funciona este _pattern_:

* [Video tutorial sobre como criar projetos MVC em consola e
  Unity][tutorial-video] (essencial).
* [MVCExample: código desse mesmo tutorial][tutorial-code].
* [ColorShapeLinks competition][ia-simplexity]: neste projeto o modelo é
  completamente independente do Unity ou da consola, existindo versões concretas
  em ambos os sistemas.
* [Jogo do Galo em consola implementado com MVC][ttt-mvc] (apenas com matéria
  de LP1, há coisas que podem ser melhoradas com matéria de LP2).
* [Solução do exercício PlayerManager4 de LP1 em MVC][pm4] (apenas consola e
  apenas usando matéria de LP1; neste caso o modelo é apenas uma lista de
  jogadores).

### Sugestões para o uso de sub-módulos em Git

Para fazerem _clone_ de um projeto com sub-módulos devem usar o seguinte
comando, exemplificado para o projeto exemplo [MVCExample][tutorial-code]:

```
git clone --recurse-submodules https://github.com/VideojogosLusofona/MVCExample.git
```

Caso se tenham esquecido de usar a opção `--recurse-submodules`, podem executar
o seguinte comando na raiz do projeto que obtém os conteúdos dos sub-módulos:

```
git submodule update --init --recursive
```

Os sub-módulos estão inicialmente no estado `HEAD detached`, isto é, não estão
em nenhum ramo. Para os sub-módulos ficarem no ramo pretendido, por exemplo o
ramo `common`, basta fazer `cd` até à pasta de cada sub-módulo e fazer
`git checkout common` (e depois `git pull` para obter as últimas alterações
ou `git add/commit/push` para criarem _commits_ específicos ao sub-módulo).

<!-- Em
alternativa podem obter as últimas alterações no ramo `common`

git submodule foreach --recursive git checkout common-->

O projeto [ColorShapeLinks][ia-simplexity] usa esta estratégia.

Como alternativa a um ramo separado, podem usar para o vosso projeto um segundo
repositório para conter o código comum.

### Sugestão sobre como começar a abordar este projeto

Em primeiro lugar devem criar o jogo fisicamente, incluindo tabuleiro e peças
jogáveis, e fazer alguns jogos. Esta abordagem inicial permite desenvolver uma
melhor noção do jogo, facilitando o posterior planeamento e implementação em C#.

Antes de começarem a preocupar-se com a arquitetura do projeto, MVC e submódulos
em Git, sugiro que implementem o jogo em consola e/ou Unity tendo como
única preocupação ver o mesmo a funcionar (não se esqueçam de usar Git logo
de início, pois todos os _commits_ contam).

Quando tiverem confiança que o jogo está bem implementado, tentem separar todo
o código específico da que defina a lógica e estado do jogo (que não contenha
código de consola, e.g. `Console.QualquerCoisa()`, nem código de Unity, e.g.
`using UnityEngine`). Esse código é o **modelo** (o **M** em MVC), e deve ser
colocado num projeto/pasta/_namespace_ próprio, e posteriormente num ramo (ou
repositório) próprio, tal como apresentado no [tutorial][tutorial-video]. Nesta
fase podem implementar a outra versão que ainda não implementaram, usando esse
código comum do modelo, tal como descrito no tutorial. Pode ser necessário
eventualmente fazer algum ajuste ao código comum. Devem garantir que tanto a
versão de consola como a versão Unity têm sempre o código comum igual e
atualizado.

## Objetivos e critério de avaliação

O projeto tem um peso de 10 valores na componente prática de LP2. A nota final
do projeto será atribuída segundo os seguintes objetivos:

* **O1** - Programa deve funcionar como especificado. Atenção aos detalhes,
  pois é fácil desviarem-se das especificações caso não leiam o enunciado com
  atenção.
* **O2** - Projeto e código bem organizados, nomeadamente:
  * O projeto deve estar devidamente organizado, fazendo uso de classes,
    `struct`s e/ou enumerações, consoante seja mais apropriado. Cada tipo
    (i.e., classe, `struct` ou enumeração) deve ser colocado num ficheiro com o
    mesmo nome. Por exemplo, uma classe chamada `Ghost` deve ser colocada
    no ficheiro `Ghost.cs`.
  * A escolha da coleções e *design patterns* deve ser adequada a cada
    situação. Serão privilegiadas soluções que tenham em consideração bons
    princípios de design de classes, como é o caso dos princípios [SOLID].
    Estes *patterns* e princípios devem ser balanceados com o princípio
    [KISS], crucial no desenvolvimento de qualquer aplicação. O único _pattern_
    obrigatório é mesmo o MVC.
  * O código deve ser o mais eficiente possível sem comprometer a arquitetura.
  * O código deve estar devidamente comentado e indentado.
  * Não deve existir código "morto", que não faz nada, como por exemplo
    variáveis, propriedades ou métodos nunca usados.
  * Projeto compila e executa sem erros e/ou *warnings*.
* **O3** - Projeto adequadamente documentado com [comentários de documentação
  XML][XML]. A documentação gerada em formato HTML em [Doxygen] ou [DocFX],
  deve estar incluída no `zip` do projeto, mas **não** integrada no repositório
  Git.
* **O4** - Repositório Git deve refletir boa utilização do mesmo, nomeadamente:
  * Devem existir *commits* de todos os elementos do grupo, _commits_ esses
    com mensagens que sigam as melhores práticas para o efeito (como indicado
    [aqui](https://chris.beams.io/posts/git-commit/),
    [aqui](https://gist.github.com/robertpainsi/b632364184e70900af4ab688decf6f53),
    [aqui](https://github.com/erlang/otp/wiki/writing-good-commit-messages) e
    [aqui](https://stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting)).
  * Ficheiros binários não necessários, como por exemplo todos os que são
    criados nas pastas `bin` e `obj`, bem como os ficheiros de configuração
    do Visual Studio (na pasta `.vs` ou `.vscode`), não devem estar no
    repositório. Ou seja, devem ser ignorados ao nível do ficheiro
    `.gitignore`.
  * *Assets* binários necessários, como é o caso da imagem do diagrama UML,
    devem ser integrados no repositório em modo Git LFS.
* **O5** - Relatório em formato [Markdown] (ficheiro `README.md`),
  organizado da seguinte forma:
  * Título do projeto.
  * Autoria:
    * Nome dos autores (primeiro e último) e respetivos números de aluno.
    * Informação de quem fez o quê no projeto. Esta informação é
      **obrigatória** e deve refletir os *commits* feitos no Git.
    * Indicação do repositório Git utilizado. Esta indicação é
      opcional, pois podem preferir manter o repositório privado após a
      entrega.
  * Arquitetura da solução:
    * Descrição da solução, com breve explicação de como o código foi
      organizado, bem como dos algoritmos não triviais que tenham sido
      implementados.
    * Um diagrama UML de classes simples (i.e., sem indicação dos
      membros da classe) descrevendo a estrutura de classes.
  * Referências, incluindo trocas de ideias com colegas, código aberto
    reutilizado (e.g., do StackOverflow) e bibliotecas de terceiros
    utilizadas. Devem ser o mais detalhados possível.
  * **Nota:** o relatório deve ser simples e breve, com informação mínima e
    suficiente para que seja possível ter uma boa ideia do que foi feito.
    Atenção aos erros ortográficos e à correta formatação [Markdown], pois
    ambos serão tidos em conta na nota final.

O projeto tem um peso de 10 valores na nota final da disciplina e será avaliado
de forma qualitativa. Isto significa que todos os objetivos têm de ser
parcialmente ou totalmente cumpridos. A cada objetivo, O1 a O5, será atribuída
uma nota entre 0 e 1. A nota do projeto será dada pela seguinte fórmula:

*N = 10 x O1 x O2 x O3 x O4 x O5 x D x A*

Em que *D* corresponde à nota da discussão e percentagem equitativa de
realização do projeto, também entre 0 e 1. Isto significa que se os alunos
ignorarem completamente um dos objetivos, não tenham feito nada no projeto ou
não comparecerem na discussão, a nota final será zero.

O termo *A* representa uma penalização por atrasos na entrega.

## Entrega

O projeto deve ser submetido no Moodle até às **23h00 de 29 de agosto de 2021**.
O projeto entregue deve ter os seguintes conteúdos:

* Todos os ficheiros do projeto incluídos no repositório git.
* Pasta escondida `.git` com o repositório Git local do projeto.
* Documentação HTML ou CHM gerada com [Doxygen], [DocFX] ou
  ferramenta similar.
* Ficheiro `README.md` contendo o relatório do projeto em formato [Markdown].
* Ficheiros de imagens, contendo o diagrama UML de classes e outras figuras
  que considerem úteis. Estes ficheiros, bem como ficheiros binários da versão
  Unity, devem ser incluídos no repositório em modo Git LFS.

**Atenção:** A submissão tem de ter menos de 20 Megabytes e ser efetuada mesmo
através do Moodle. Não são aceites _links_ para Google Drive, etc. Desta forma
tenham muito cuidado com o `.gitignore` e o `.gitattributes` no início do
projeto, e tenham em atenção que a versão Unity e de consola podem precisar de
ficheiros de configuração Git diferentes (ver [código do
tutorial][tutorial-code]).

## Honestidade académica

Nesta disciplina, espera-se que cada aluno siga os mais altos padrões de
honestidade académica. Isto significa que cada ideia que não seja do
aluno deve ser claramente indicada, com devida referência ao respectivo
autor. O não cumprimento desta regra constitui plágio.

O plágio inclui a utilização de ideias, código ou conjuntos de soluções
de outros alunos ou indivíduos, ou quaisquer outras fontes para além
dos textos de apoio à disciplina, sem dar o respectivo crédito a essas
fontes. Os alunos são encorajados a discutir os problemas com outros
alunos e devem mencionar essa discussão quando submetem os projetos.
Essa menção **não** influenciará a nota. Os alunos não deverão, no
entanto, copiar códigos, documentação e relatórios de outros alunos, ou dar os
seus próprios códigos, documentação e relatórios a outros em qualquer
circunstância. De facto, não devem sequer deixar códigos, documentação e
relatórios em computadores de uso partilhado.

Nesta disciplina, a desonestidade académica é considerada fraude, com
todas as consequências legais que daí advêm. Qualquer fraude terá como
consequência imediata a anulação dos projetos de todos os alunos envolvidos
(incluindo os que possibilitaram a ocorrência). Qualquer suspeita de
desonestidade académica será relatada aos órgãos superiores da escola
para possível instauração de um processo disciplinar. Este poderá
resultar em reprovação à disciplina, reprovação de ano ou mesmo suspensão
temporária ou definitiva da ULHT.

*Texto adaptado da disciplina de [Algoritmos e
Estruturas de Dados][aed] do [Instituto Superior Técnico][ist]*

## Referências

* **18 Ghosts** (2010). Retrieved from
  <https://boardgamegeek.com/boardgame/70116/18-ghosts>.
* Whitaker, R. B. (2016). **The C# Player's Guide** (3rd Edition).
  Starbound Software.
* Albahari, J. (2017). **C# 7.0 in a Nutshell**. O’Reilly Media.
* Dorsey, T. (2017). **Doing Visual Studio and .NET Code Documentation
  Right**. Visual Studio Magazine. Retrieved from
  <https://visualstudiomagazine.com/articles/2017/02/21/vs-dotnet-code-documentation-tools-roundup.aspx>.

## Licenças

Este enunciado é disponibilizado através da licença [CC BY-NC-SA 4.0].

## Metadados

* Autor: [Nuno Fachada]
* Curso:  [Licenciatura em Videojogos][lamv]
* Instituição: [Universidade Lusófona de Humanidades e Tecnologias][ULHT]

[Pedra, Papel e Tesoura]:https://pt.wikipedia.org/wiki/Pedra,_papel_e_tesoura
[MVC]:https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
[Von Neumann]:https://en.wikipedia.org/wiki/Von_Neumann_neighborhood
[RPSNetLogo]:http://ccl.northwestern.edu/netlogo/models/RockPaperScissors
[NetLogo]:http://ccl.northwestern.edu/netlogo
[distribuição de Poisson]:https://en.wikipedia.org/wiki/Poisson_distribution
[distribuição uniforme]:https://en.wikipedia.org/wiki/Uniform_distribution_(continuous)
[Random]:https://docs.microsoft.com/dotnet/api/system.random
[`NextDouble()`]:https://docs.microsoft.com/dotnet/api/system.random.nextdouble
[genPoisson]:https://en.wikipedia.org/wiki/Poisson_distribution#Generating_Poisson-distributed_random_variables
[submodules]:https://git-scm.com/book/en/v2/Git-Tools-Submodules
[ia-simplexity]:https://github.com/VideojogosLusofona/color-shape-links-ai-competition
[tutorial-video]:https://www.youtube.com/watch?v=_z_iRUjmvzE
[tutorial-code]:https://github.com/VideojogosLusofona/MVCExample.git
[ttt-mvc]:https://github.com/VideojogosLusofona/lp1_2020_galo_solucao
[pm4]:https://github.com/VideojogosLusofona/lp1_2020_aulas/tree/main/Aula12/PlayerManager4
[rps-nl]:http://ccl.northwestern.edu/netlogo/models/RockPaperScissors
[Markdown]:https://guides.github.com/features/mastering-markdown/
[Doxygen]:https://www.stack.nl/~dimitri/doxygen/
[DocFX]:https://dotnet.github.io/docfx/
[KISS]:https://en.wikipedia.org/wiki/KISS_principle
[XML]:https://docs.microsoft.com/dotnet/csharp/codedoc
[SOLID]:https://en.wikipedia.org/wiki/SOLID
[CC BY-NC-SA 4.0]:https://creativecommons.org/licenses/by-nc-sa/4.0/
[lamv]:https://www.ulusofona.pt/licenciatura/videojogos
[Nuno Fachada]:https://github.com/fakenmc
[ULHT]:https://www.ulusofona.pt/
[aed]:https://fenix.tecnico.ulisboa.pt/disciplinas/AED-2/2009-2010/2-semestre/honestidade-academica
[ist]:https://tecnico.ulisboa.pt/pt/
