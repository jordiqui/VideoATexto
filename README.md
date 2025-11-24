# VideoATexto

Convierte videos de reuniones y/o clases en formato MP4 a texto usando el servicio de Speech-to-Text de Google Cloud. Google Cloud ofrece 300 USD de crédito inicial y, posteriormente, 1 hora gratuita de transcripción al mes.

## Prerrequisitos

1. Contar con una cuenta y la consola de Google Cloud.
2. Habilitar el servicio de Speech-to-Text en el proyecto de Google Cloud.
3. Crear la clave del API para consumir el servicio y configurar las credenciales en tu entorno.
4. Instalar **ffmpeg** y asegurarte de que esté en el `PATH` de tu sistema.
   - En Ubuntu/Debian:
     ```bash
     sudo apt-get install ffmpeg
     ```
   - En macOS con Homebrew:
     ```bash
     brew install ffmpeg
     ```
5. Instalar las dependencias de Python del proyecto, por ejemplo con `pip install -r requirements.txt` si el archivo existe, o instalando directamente las librerías necesarias como `moviepy`.

## Compilación en Windows (Clang o MinGW-w64)

Si deseas generar ejecutables nativos en Windows, puedes hacerlo tanto con el compilador de LLVM/Clang como con MinGW-w64. A continuación se detallan ambos flujos:

### Opción A: Usar LLVM/Clang
1. Instala [LLVM](https://releases.llvm.org/download.html) o usa el instalador oficial de Clang para Windows y marca la casilla para agregar Clang al `PATH`.
2. Instala [CMake](https://cmake.org/download/) y, opcionalmente, [Ninja](https://github.com/ninja-build/ninja/releases) para obtener compilaciones rápidas.
3. Abre una terminal de **x64 Native Tools** (o PowerShell) y verifica que Clang esté disponible:
   ```powershell
   clang --version
   ```
4. Genera el proyecto (asumiendo que usas CMake para los componentes nativos del repositorio):
   ```powershell
   cmake -S . -B build -G "Ninja" -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++
   ```
   Si prefieres las herramientas de Visual Studio, puedes omitir `-G "Ninja"` y abrir el proyecto con el generador correspondiente.
5. Compila:
   ```powershell
   cmake --build build --config Release
   ```
6. El ejecutable resultante quedará en `build` (o en el subdirectorio `Release` si el generador lo usa).

### Opción B: Usar MinGW-w64
1. Instala [MSYS2](https://www.msys2.org/) y, desde la terminal **MSYS2 UCRT64** o **MSYS2 MINGW64**, ejecuta:
   ```bash
   pacman -Syu
   pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-clang mingw-w64-x86_64-cmake mingw-w64-x86_64-ninja
   ```
   Esto instala MinGW-w64, Clang opcional, CMake y Ninja dentro del entorno MSYS2.
2. Añade el binario de MinGW-w64 al `PATH` de Windows (por ejemplo, `C:\msys64\mingw64\bin`).
3. Verifica el compilador:
   ```bash
   x86_64-w64-mingw32-gcc --version
   ```
4. Genera el proyecto con CMake usando el generador de MinGW:
   ```bash
   cmake -S . -B build -G "MinGW Makefiles"
   ```
   Para forzar Clang dentro de MinGW-w64 reemplaza los compiladores:
   ```bash
   cmake -S . -B build -G "MinGW Makefiles" -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++
   ```
5. Compila:
   ```bash
   cmake --build build --config Release
   ```
6. El ejecutable quedará en `build` (o en `build/Release` si el generador lo crea).

### Notas adicionales
- Si el proyecto incluye código Python, puedes combinar estas compilaciones con herramientas como `pyinstaller` para generar binarios que integren el intérprete y las dependencias.
- Asegúrate de que `ffmpeg` esté en el `PATH` del entorno en el que ejecutes el binario resultante.
- Mantén las credenciales de Google Cloud configuradas (variables de entorno o archivo de credenciales) en el mismo entorno donde ejecutes el programa.

Con estos pasos tendrás soporte para compilar en Windows con Clang o MinGW-w64 y podrás ofrecer ejecutables a los usuarios directamente desde GitHub.
