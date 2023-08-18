Fala pessoal, neste post irei mostrar duas formas de lidar com eventos do tipo onChange, para o exemplo irei utilizar o input do tipo checkbox, mas também se aplica para os outros tipos de input, desde que tenha o evento **onChange** atrelado.

Basicamente iremos definir um componente para o input do tipo checkbox, onde iremos atribuir ao mesmo um useState do tipo boolean onde o value representa o valor para “checked”  e o setValue irá ser a função de atribuir valor no momento que o evento onChange estiver sendo chamado. 

Após isso iremos criar uma função que será do tipo **React.ChangeEventHandler<HTMLInputElement>** que irá receber o evento e pegar o **currentTarget.checked**. 


{% gist bfe3339881afcec2e5cb51f5f3f6c78c %}



Da forma apresentada acima a chamada da função que irá manipular o evento ficar de forma separada, entretanto se você necessitar apenas do valor que está sendo atribuído podemos realizar a atribuição de valor de uma forma mais simples, chamando a função de forma inline onde não precisamos especificar o tipo de parâmetro recebido ou o tipo que a função está atrelada. Desta forma o typescript irá aferir o tipo automaticamente e teremos o mesmo resultado do exemplo anterior.

{% gist 8edb1ff5157f3b86b30aecb3daa228eb %}
