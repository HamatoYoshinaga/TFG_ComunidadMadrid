INICIO

    // --- 1. Cargar datos ---
    LEER GeoDataFrame Municipios  (ID_mun, Nombre_mun, Geometría_polígono)
    LEER GeoDataFrame Campus      (ID_camp, Nombre_camp, Geometría_polígono)

    // --- 2. Calcular centroides ---
    PARA CADA municipio EN Municipios HACER
        municipio.Centroide ← CALCULAR_CENTROIDE(municipio.Geometría_polígono)
    FIN PARA

    PARA CADA campus EN Campus HACER
        campus.Centroide ← CALCULAR_CENTROIDE(campus.Geometría_polígono)
    FIN PARA

    // --- 3. Calcular distancias euclídeas ---
    CREAR Diccionario_Relaciones  // clave: ID_municipio, valor: lista de (ID_campus, distancia)

    PARA CADA municipio EN Municipios HACER
        CREAR Lista_Distancias ← VACÍA

        PARA CADA campus EN Campus HACER
            distancia ← CALCULAR_DISTANCIA_EUCLIDEA(municipio.Centroide, campus.Centroide)
            AÑADIR (campus.ID_camp, distancia) A Lista_Distancias
        FIN PARA

        // --- 4. Seleccionar los 3 campus más cercanos ---
        ORDENAR Lista_Distancias por distancia ASCENDENTE
        Seleccion ← PRIMEROS_3(Lista_Distancias)

        // Guardar en diccionario
        Diccionario_Relaciones[municipio.ID_mun] ← Seleccion
    FIN PARA

    // --- 5. Devolver resultados ---
    DEVOLVER Diccionario_Relaciones

FIN

El diccionario relaciones tendrá la forma:
{
   ID_municipio1: [(ID_campusA, distA), (ID_campusB, distB), (ID_campusC, distC)],
   ID_municipio2: [(ID_campusX, distX), (ID_campusY, distY), (ID_campusZ, distZ)],
   ...
}