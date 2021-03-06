FindITKUtil(BRAINSSurfaceTools_ITK
  ITKQuadEdgeMesh
  ITKOptimizers
  ITKQuadEdgeMeshFiltering
)

FindVTKUtil(vtkIOLegacy
  vtkIOXML
  vtkCommonDataModel
  vtkFiltersCore
  vtkFiltersGeneral
  vtkFiltersGeometry
  vtkImagingCore
  vtkFiltersExtraction
  )


StandardBRAINSBuildMacro(NAME BRAINSAssignSurfaceFeatures
  TARGET_LIBRARIES BRAINSCommonLib
  ${BRAINSSurfaceTools_ITK_LIBRARIES}
  ${VTK_LIBRARIES})

## Missing data files
if (FALSE AND BUILD_TESTING AND NOT BRAINSTools_DISABLE_TESTING)

set(BRAINSAssignSurfaceFeaturesProg ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/BRAINSAssignSurfaceFeatures)
set(CompareSurfacesProg ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/CompareSurfaces)

ExternalData_add_test( ${BRAINSTools_ExternalData_DATA_MANAGEMENT_TARGET} NAME TEST_BRAINSAssignSurfaceFeatures
  COMMAND ${LAUNCH_EXE} $<TARGET_FILE:BRAINSAssignSurfaceFeatures>
          --inputSurfaceFile DATA{${TestData_DIR}/0036195_SurfaceGeneration.vtk}
          --outerSurfaceFile DATA{${TestData_DIR}/0036195_leftTissueClass130.vtk}
          --outputSurfaceFile 0036195_SurfaceFeaturesTest.vtk
          --distanceToPC_AP
          --distanceToPC_IS
          --PC -129.5,-178.75,127.97
          --distanceToHull
          --corticalThickness
          --maxThickness 7.0
          --curvature
          --curvatureType Mean )

ExternalData_add_test( ${BRAINSTools_ExternalData_DATA_MANAGEMENT_TARGET} NAME COMPARE_BRAINSAssignSurfaceFeatures
  COMMAND ${LAUNCH_EXE} $<TARGET_FILE:CompareSurfaces>
          --inputSurfaceFile 0036195_SurfaceFeaturesTest.vtk
          --refSurfaceFile DATA{${TestData_DIR}/0036195_leftSurfaceFeatures.vtk}
          --vertexLocation
          --scalarArray
          --tolerance 1.0e-7 )

set_tests_properties( COMPARE_BRAINSAssignSurfaceFeatures PROPERTIES DEPENDS TEST_BRAINSAssignSurfaceFeatures )

endif ()
