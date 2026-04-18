Práctica de Comandos Linux en Terminal
# 1. Título
Gestión de Archivos y Directorios mediante Comandos CLI en GitBash

# 2. Tiempo de duración
45 minutos

# 3. Fundamentos
La terminal CLI (Command Line Interface) en sistemas UNIX-like proporciona acceso directo al kernel Linux mediante comandos que invocan syscalls del sistema. GitBash en Windows emula un entorno POSIX sobre MinGW, mapeando comandos Linux al subsistema Windows NT mediante una capa de compatibilidad cygwin-like. El kernel Linux gestiona el sistema de archivos jerárquico mediante inodos, que almacenan metadatos (permisos, timestamps, ownership) y apuntan a bloques de datos físicos.

Figura 3-1. Estructura del sistema de archivos Linux con inodos

Los comandos de manipulación de archivos interactúan con syscalls específicas:

mkdir invoca mkdirat() para crear directorios con permisos predeterminados 755 (rwxr-xr-x)

cp utiliza clonefile() o read()+write() para duplicar contenido preservando atributos

mv ejecuta renameat2() para renombrar/mover atómicamente dentro del mismo filesystem

rm llama unlinkat() eliminando enlaces duros sin tocar datos hasta que pierden referencias

Figura 3-2. Flujo de descriptores de archivo estándar (stdin=0, stdout=1, stderr=2)

El redireccionamiento modifica descriptores de archivo mediante operadores shell:

> ejecuta dup2(stdout, fd_nuevo) sobrescribiendo archivos con O_TRUNC

>> usa O_APPEND posicionando el offset al final antes de escribir

history > archivo captura el buffer de comandos del shell actual

La shell Bash mantiene un buffer circular de comandos (histfile ~/.bash_history, HISTSIZE=500), accesible mediante history [n]. Esta práctica desarrolla competencias esenciales para DevOps, donde la automatización vía scripts shell reemplaza clics manuales de GUI, habilitando despliegues reproducibles en pipelines CI/CD.

El sistema de archivos FAT32/NTFS de Windows se mapea a paths Unix-style en GitBash (/c/Users/...), preservando case-sensitivity solo en nombres. La ausencia de papelera de reciclaje en rm fomenta disciplina en operaciones destructivas, práctica estándar en servidores Linux productivos.

Figura 3-3. Comparativa CLI vs GUI en operaciones de archivos

Dominar estos comandos es prerequisite para tecnologías modernas como Docker (docker run -v), Ansible (file modules) y Kubernetes (configmaps), donde interfaces gráficas no existen y la precisión del comando determina el éxito del despliegue.


# 4. Conocimientos previos
Para realizar esta práctica el estudiante necesita tener claro los siguientes temas:

Comandos básicos Linux: mkdir, cp, mv, rm, ls, cd, pwd

Navegación en terminal GitBash desde Windows

Conceptos de permisos de archivo UNIX (rwxrwxrwx)

Edición básica con cat, echo

Redireccionamiento de salida (>, >>)

Estructura de directorios jerárquicos

# 5. Objetivos a alcanzar
Crear estructura de directorios anidados mediante mkdir

Manipular archivos con cp, mv y rm corrigiendo errores tipográficos

Implementar redireccionamiento de salida con > y >>

Generar historial de comandos para auditoría

Desarrollar autonomía en terminal eliminando dependencia de GUI Windows Explorer

Validar operaciones mediante inspección de filesystem

# 6. Equipo necesario
PC con Windows 11 Pro/Education (WSL2 opcional)

GitBash v2.41.0.windows.3+ (incluye MinGW64, BusyBox)

4GB RAM libre, 500MB espacio en disco C:

Editor de texto (Notepad++, VS Code)

Terminal moderna (Windows Terminal recomendado)

# 7. Material de apoyo
Cheat sheet Linux commands: linuxcommands.org

Documentación Git for Windows: gitforwindows.org

Guía comandos básicos Bash: tldp.org/LDP/abs/html/

Práctica anterior: Fundamentos teóricos Linux CLI

Referencia redireccionamiento: bash-hackers.org/cmds/all.html

# 8. Procedimiento
Paso 1: Inicializar entorno de trabajo

bash
C:\Users\vd236\Documents\
mkdir proyecto_comandos
cd proyecto_comandos
pwd
Figura 8-1. Verificación directorio de trabajo inicial

Paso 2: Crear estructura de directorios

bash
mkdir documentos imagenes scripts
Error tipográfico intencional:
mkdir coumentos  # INCORRECTO
ls -la
Paso 3: Corregir error con mv

bash
mv coumentos documentos
ls -la
Figura 8-2. Corrección de nombre de directorio mediante mv

Paso 4: Crear y manipular archivos

bash
cd documentos
cat > notas.txt << 'EOF'
Comando mkdir: crea directorios
Comando cp: copia archivos
Comando mv: mueve/renombra
Comando rm: elimina archivos
EOF

cp notas.txt ../scripts/backup_notas.txt
mv notas.txt ../imagenes/
ls -la ../*
Paso 5: Implementar redireccionamiento

bash
cd ..
cat ../imagenes/notas.txt > resumen.txt
echo "Operaciones completadas exitosamente" >> resumen.txt
cat resumen.txt
Figura 8-3. Flujos de redireccionamiento stdout a archivos

Paso 6: Limpieza de archivos y directorios

bash
rm ../scripts/backup_notas.txt
ls ../scripts/
rmdir ../imagenes/
ls -la ..
Paso 7: Generar historial de sesión

bash
history > tarea-s1-Valeria_Diaz.txt
tail -10 tarea-s1-Valeria_Diaz.txt
Figura 8-4. Historial de comandos de la sesión completa

# 9. Resultados esperados
Estructura final en C:\Users\vd236\Documents\proyecto_comandos

text
proyecto_comandos/
├── documentos/ (vacío)
├── scripts/ (vacío)
├── resumen.txt (4 líneas con contenido)
└── tarea-s1-Valeria_Diaz.txt (historial completo)
Figura 9-1. Estado final del filesystem tras todas las operaciones

Figura 9-2. Contenido del archivo resumen.txt generado

Figura 9-3. Últimas líneas del historial de comandos

Verificación exitosa de todas las operaciones sin pérdida de datos críticos. El estudiante demuestra competencia en manejo CLI eliminando dependencia total de GUI.

# 10. Bibliografía


The Linux Documentation Project. (2025). Bash Guide for Beginners. Recuperado de http://tldp.org/LDP/Bash-Beginners-Guide/html/

Git for Windows. (2026). Git Bash documentation. Recuperado de https://gitforwindows.org

Robbins, A. (2023). Learning the bash Shell (4th ed.). O'Reilly Media.
