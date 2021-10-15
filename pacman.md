# PACMAN

## Instalar paquetes
- pacman -S “paquete” #Instala un paquete.
- pacman -Sy “paquete” #Sincroniza repositorios e instala el paquete.

## Actualizar paquetes
- pacman -Sy #Sincroniza repositorios.
- pacman -Syy #Fuerza la sincronización de repositorios incluso para paquetes que parecen actualizados.
- pacman -Syu #Sincroniza repositorios y actualiza paquetes.
- pacman -Syyu #Fuerza sincronización y actualiza paquetes.
- pacman -Su #Actualiza paquetes sin sincronizar repositorios.

## Buscar paquetes
- pacman -Ss “paquete” #Busca un paquete.
- pacman -Si “paquete” #Muestra información detallada de un paquete.
- pacman -Sg “grupo” #Lista los paquetes que pertenecen a un grupo.
- pacman -Qs “paquete” #Busca un paquete YA instalado.
- pacman -Qi “paquete” #Muestra información detallada de un paquete YA instalado.
- pacman -Qdt #Muestra paquetes huerfanos.

## Eliminar paquetes
- pacman -R “paquete” #Borra paquete sin sus dependencias.
- pacman -Rs “paquete” #Borra paquete y sus dependencias no utilizadas.
 
## Listar paquetes instalados explicitamente
- pacman -Qqe

## Identificar archivos que no pertenecen a ningún paquete
- find /etc /usr /opt /var | LC_ALL=C pacman -Qqo - 2>&1 > /dev/null | cut -d ' ' -f 5-

## Eliminar paquetes no utilizados (huérfanos)
- pacman -Rns $(pacman -Qtdq)
