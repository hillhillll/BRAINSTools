StandardBRAINSBuildMacro(NAME WarpQuadEdgeMesh TARGET_LIBRARIES BRAINSCommonLib ${BRAINSSurfaceTools_ITK_LIBRARIES} ${VTK_LIBRARIES})

if (BUILD_TESTING AND NOT BRAINSTools_DISABLE_TESTING)

ExternalData_add_test( ${BRAINSTools_ExternalData_DATA_MANAGEMENT_TARGET} NAME TEST_SurfaceWarpQuadEdgeMesh
  COMMAND ${LAUNCH_EXE} $<TARGET_FILE:WarpQuadEdgeMesh>
          --fixedMeshFile DATA{${TestData_DIR}/0036195_left_sphere.vtk}
          --movingMeshFile DATA{${TestData_DIR}/0036896_left_sphereIC6.vtk}
          --deformedMeshFile DATA{${TestData_DIR}/deformed0036195.vtk}
          --outputMeshFile warped0036896HullTest.vtk)

ExternalData_add_test( ${BRAINSTools_ExternalData_DATA_MANAGEMENT_TARGET} NAME COMPARE_SurfaceWarpQuadEdgeMesh
  COMMAND ${LAUNCH_EXE} $<TARGET_FILE:CompareSurfaces>
          --inputSurfaceFile warped0036896HullTest.vtk
          --refSurfaceFile DATA{${TestData_DIR}/warped0036896Hull.vtk}
          --vertexLocation
          --scalarArray
          --tolerance 1.0e-7 )

set_tests_properties( COMPARE_SurfaceWarpQuadEdgeMesh PROPERTIES DEPENDS TEST_SurfaceWarpQuadEdgeMesh )

endif ()
