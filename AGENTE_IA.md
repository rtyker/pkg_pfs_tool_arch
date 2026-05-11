# Instrucoes Para Agente de IA

Objetivo: reconstruir localmente o workspace do projeto `pkg_pfs_tool`, aplicar os ajustes minimos de build e validar a compilacao.

## Repositorio upstream

- URL: `https://github.com/flatz/pkg_pfs_tool.git`

## Procedimento esperado

1. Clonar o repositorio.
2. Verificar `README.md` e `CMakeLists.txt`.
3. Garantir que o `CMakeLists.txt` use:
   - `include_directories(${MBEDTLS_INCLUDE_DIR})`
   - `include_directories(${UTHASH_INCLUDE_DIR})`
4. Garantir que o alvo `pkg_pfs_tool` tenha um `POST_BUILD` copiando `config.ini` para o diretorio do executavel:

```cmake
add_custom_command(TARGET pkg_pfs_tool POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${CMAKE_SOURCE_DIR}/config.ini"
		"$<TARGET_FILE_DIR:pkg_pfs_tool>/config.ini"
)
```

5. Rodar:

```bash
rm -rf build
cmake -S . -B build
cmake --build build -j$(nproc)
./build/pkg_pfs_tool --help
```

## Motivo tecnico do patch

Sem o ajuste das variaveis de include, o build pode acabar usando headers do `mbedtls` instalados no sistema, que em versoes mais novas possuem API incompativel com o codigo esperado pelo projeto. O sintoma observado foi erro em `mbedtls_rsa_init`.

## Resultado esperado

- Binario gerado em `build/pkg_pfs_tool`
- `config.ini` presente em `build/config.ini`
- Ajuda do programa exibida com sucesso

## Observacoes

- O build passou neste ambiente Linux sem alterar os arquivos `.c`.
- Ainda existem warnings de `snprintf` e outros warnings menores; eles nao impediram a compilacao.
