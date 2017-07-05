SCENARIO := SampleSimulation
#
MKDIR_P	:= mkdir -p
RM_RF	:= rm -rf
RM_F	:= rm -f
#
DOCKER	:= docker
DOCKERFILE := Dockerfile
LOCAL_IMAGE := gatling:local
# FOLDERS
CONF	:= conf
RESULTS	:= results
USER-FILES :=user-files
SIMULATIONS := ${USER-FILES}/simulations
DATA 	:= ${USER-FILES}/data

setup:	prepare dockerfile build

prepare:
	${MKDIR_P} ${CONF} ${RESULTS} ${SIMULATIONS} ${DATA}

dockerfile:
	echo "FROM tsamaya/docker-gatlingio:latest" > ${DOCKERFILE}
.PHONY:	dockerfile

build:	dockerfile
	${DOCKER} build -t ${LOCAL_IMAGE} .

run_it:	setup
	${DOCKER} run -it --rm -v `pwd`/conf:/opt/gatling/conf -v `pwd`/user-files:/opt/gatling/user-files -v `pwd`/results:/opt/gatling/results -e JAVA_OPTS=${JAVA_OPTS} ${LOCAL_IMAGE}

run:	setup
	${DOCKER} run --rm -v `pwd`/conf:/opt/gatling/conf -v `pwd`/user-files:/opt/gatling/user-files -v `pwd`/results:/opt/gatling/results -e JAVA_OPTS=${JAVA_OPTS} ${LOCAL_IMAGE} -s ${SCENARIO}

clean:
	${RM_RF} ${CONF} ${RESULTS} ${USER-FILES}

cleanres:
	${RM_RF} ${RESULTS}/*

mrproper: clean
	${RM_F} ${DOCKERFILE}
