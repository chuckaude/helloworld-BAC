node {
	withCoverityEnv(coverityToolName: '2018.06', connectInstance: 'localhost') {
		STREAM = 'hello'
		IDIR = 'idir'
		stage('build') {
			sh "cov-build --dir ${IDIR} make clean all"
		}
		stage('analyze') {
			sh "cov-analyze --dir ${IDIR} --strip-path ${WORKSPACE} --all --enable-callgraph-metrics --enable-fnptr --enable-virtual"
		}
		stage('commit') {
			sh "cov-commit-defects --dir ${IDIR} --host ${COVERITY_HOST} --stream ${STREAM} --scm git --description ${BUILD_TAG} --target Linux_x86_64 --version ${GIT_COMMIT}"
		}
	}
	stage('results') {
		coverityResults connectInstance: 'localhost', connectView: 'High Impact Outstanding', projectId: 'hello'
	}
}
