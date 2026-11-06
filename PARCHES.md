# Parches

Este repo es OJS oficial con nuestros parches encima. Cada versión tiene su rama, por ejemplo `glaux/3_5_0-6`.

Repos:

- `Glaux-PA/ojs`
- `Glaux-PA/pkp-lib`, que es el submódulo `lib/pkp`
- `Glaux-PA/omp`

Lo que se cambia en `pkp-lib` afecta a OJS y a OMP.

## Parches en 3_5_0-6

| Parche | Dónde | Afecta a |
|---|---|---|
| La vista Rechazadas del panel sale también a subeditores y asistentes | `pkp-lib`: `classes/submission/Repository.php` | OJS y OMP |
| Selector de revisión al cambiar archivo en formatos de publicación (pkp-lib#13416) | `pkp-lib`: `PKPApplication.php` y `PKPSubmissionFilesUploadBaseForm.php`. `omp`: `classes/core/Application.php` | Solo OMP |
| Traducción al español de `invitation.po` y `sushi.po` | `ojs`: `locale/es`. `pkp-lib`: `locale/es/sushi.po` | OJS y OMP |

Si PKP arregla un fallo en una versión nueva, se quita de la tabla.

## Añadir un parche

Si el fallo está en `lib/pkp`:

```
cd lib/pkp
git add .
git commit -m "Texto del commit"
git push origin glaux/3_5_0-6
cd ../..
git add lib/pkp
git commit -m "Actualizado pkp-lib"
git push origin glaux/3_5_0-6
```

Si el fallo está en `ojs`, commit y push normal. Si afecta a OMP, hay que repetir el último paso en el repo `omp`.

Un commit por parche. Después se apunta en la tabla.

## Pasar a una versión nueva

Con `3_5_0-7` como ejemplo.

```
git fetch upstream --tags
git -C lib/pkp fetch upstream --tags
git checkout -b glaux/3_5_0-7 3_5_0-7
git submodule update --init --recursive
cd lib/pkp
git checkout -b glaux/3_5_0-7
```

Se mira qué parches siguen sin arreglar y se copian con cherry-pick:

```
git cherry-pick <commit>
git push -u origin glaux/3_5_0-7
cd ../..
```

En `ojs` se cambia la URL de `lib/pkp` en `.gitmodules` a `https://github.com/Glaux-PA/pkp-lib.git`, se copian los parches de `ojs` y se sube:

```
git add .gitmodules lib/pkp
git commit -m "Apuntado lib/pkp al fork de Glaux-PA"
git push -u origin glaux/3_5_0-7
```

Lo mismo en `omp` y se actualiza la tabla.
