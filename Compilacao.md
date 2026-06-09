# IOT - Resumo Compilação

Primeira coisa a ser feita é rodar o comando `ipconfig` no terminal do windows. Após isso, pegar o número do Endereço IPv4: `xxx.xxx.xx.x`, por exemplo.
Após ter essa informação, é necessário acessar os arquivos `astrotrack_esp32\astrotrack_esp32.ino ` e `dashboard\index.html`, procurar pela linha 22 em `astrotrack_esp32\astrotrack_esp32.ino ` e linha 235 em `dashboard\index.html` e alterar pelos números coletados
Próximos passos:
- Verfique se a placa está identifica corretamente (esp32 DevModule)
	- Se necessário baixar a board esp32 no menu lateral.
- Verifique se todas as extensões necessárias estão instaladas
	- LiquidCrystal_I2C
- No meno superior clique em ´Sketch´.
- Por fim em ´Export Compiled Binary´ e espera a mensagem de 'Done'.

Abra o VsCode na pasta do raiz do projeto. Faça login com a Wokwi para pegar a licença. Ao terminar a configuração do Wokwi, abra o terminal e inicie a API Mock com JSON-Server. `json-server --watch dashboard/db.json --port 3000 --host 0.0.0.0`.
Após iniciar o Server, abra o arquivo `index.html`, clique com o botão direito do mouse e vá para `Open with Live Server`.
Pronto! Basta apresentar o projeto!
