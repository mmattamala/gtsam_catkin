cmake_minimum_required(VERSION 3.1)
project(gtsam_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)
file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(gtsam_src
  GIT_REPOSITORY git@github.com:borglab/gtsam.git
  GIT_TAG origin/develop
  GIT_PROGRESS ON
  CONFIGURE_COMMAND cmake -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
                          -DGTSAM_USE_SYSTEM_EIGEN=ON
                          -DGTSAM_WITH_EIGEN_MKL=OFF
                          -DGTSAM_WITH_TBB=OFF
                          -DGTSAM_BUILD_DOCS=OFF
                          -DGTSAM_BUILD_DOC_HTML=OFF
                          -DGTSAM_BUILD_EXAMPLES_ALWAYS=OFF
                          -DGTSAM_UNSTABLE_BUILD_PYTHON=OFF
                          -DCMAKE_BUILD_TYPE=Release ../gtsam_src
  BUILD_COMMAND make -j8
  INSTALL_COMMAND make install
)

install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/CppUnitLite
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/gtsam
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/gtsam_unstable
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")

install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        FILES_MATCHING PATTERN "libCppUnitLite*")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        FILES_MATCHING PATTERN "libgtsam*")
        install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        FILES_MATCHING PATTERN "libmetis-gtsam*")

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
          LIBRARIES CppUnitLite gtsam gtsam_unstable metis-gtsam)
