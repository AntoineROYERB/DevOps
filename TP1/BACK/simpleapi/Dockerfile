# Build
# Utilise une image Maven avec Amazon Corretto 17 comme base
FROM maven:3.8.6-amazoncorretto-17 AS myapp-build
# Définit une variable d'environnement MYAPP_HOME pour le répertoire de l'application
ENV MYAPP_HOME /opt/myapp
# Définit le répertoire de travail actuel à MYAPP_HOME
WORKDIR $MYAPP_HOME
# Copie le fichier de configuration Maven (pom.xml) du répertoire local à l'image
COPY pom.xml .
# Copie le code source de l'application depuis le répertoire local à l'image
COPY src ./src
# Utilise Maven pour construire l'application, en sautant l'exécution des tests
RUN mvn package -DskipTests

# Run
# Utilise une image Amazon Corretto 17 comme base
FROM amazoncorretto:17
# Réutilise la variable d'environnement MYAPP_HOME pour le répertoire de l'application (peut être redondant)
ENV MYAPP_HOME /opt/myapp
# Définit le répertoire de travail actuel à MYAPP_HOME (peut être redondant)
WORKDIR $MYAPP_HOME
# Copie le fichier JAR généré lors de l'étape de construction depuis l'étape précédente à l'image
COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar
# Définit la commande d'entrée pour l'image Docker
ENTRYPOINT java -jar myapp.jar
