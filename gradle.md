# Tips de gradle

## Mostrar la consola en el output
En build scripts en el archivo build.gradle agregar:
```
test {
    testLogging.showStandardStreams = true
}
```