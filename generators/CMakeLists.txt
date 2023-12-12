SET(GLD_MODULE_NAME generators)

if(GLD_USE_FRUGALLY)
	find_package(frugally-deep CONFIG REQUIRED)
	find_package(Eigen3 CONFIG REQUIRED)
	find_package(nlohmann_json CONFIG REQUIRED)
	find_package(FunctionalPlus CONFIG REQUIRED)
	SET(FRUGALLY_DEEP_LIBS frugally-deep::fdeep FunctionalPlus::fplus Eigen3::Eigen nlohmann_json::nlohmann_json)
endif()

add_library(${GLD_MODULE_NAME}
        battery.cpp
        battery.h
        central_dg_control.cpp
        central_dg_control.h
        controller_dg.cpp
        controller_dg.h
        diesel_dg.cpp
        diesel_dg.h
        energy_storage.cpp
        energy_storage.h
        generators.h
        init.cpp
        inverter.cpp
        inverter.h
        inverter_dyn.cpp
        inverter_dyn.h
        ibr_gfl.cpp
        ibr_gfl.h
        ibr_gfm_vsm.cpp
        ibr_gfm_vsm.h
        ibr_gfm_drp.cpp
        ibr_gfm_drp.h
        ibr_blackbox.cpp
        ibr_blackbox.h
        ibr_graybox.cpp
        ibr_graybox.h
	ibr_skeleton.cpp
	ibr_skeleton.h
        main.cpp
        rectifier.cpp
        rectifier.h
        sec_control.cpp
        sec_control.h
        solar.cpp
        solar.h
        sync_ctrl.cpp
        sync_ctrl.h
        windturb_dg.cpp
        windturb_dg.h
	cblock.cpp
	cblock.h
        )

set_target_properties(${GLD_MODULE_NAME} PROPERTIES PREFIX "")

target_compile_options(${GLD_MODULE_NAME} PRIVATE ${GLD_COMPILE_OPTIONS})
set_target_properties(${GLD_MODULE_NAME} PROPERTIES PREFIX "")
target_include_directories(${GLD_MODULE_NAME} PRIVATE "${CMAKE_SOURCE_DIR}/gldcore")
target_link_libraries(${GLD_MODULE_NAME} PRIVATE ${OS_SPECIFIC_LIBRARIES} ${GLD_VCPKG_DEPS})
target_link_libraries(${GLD_MODULE_NAME} INTERFACE ${FRUGALLY_DEEP_LIBS})
#target_include_directories(${GLD_MODULE_NAME} PRIVATE ${VCPKG_INSTALL_DIR}/${VCPKG_TARGET_TRIPLET}/include)

if(frugally-deep_FOUND)
	target_compile_definitions(${GLD_MODULE_NAME} PRIVATE HAVE_FRUGALLY)
endif()

if (GLD_USE_CLANG_TIDY)
    set_target_properties(${GLD_MODULE_NAME} PROPERTIES
            CXX_CLANG_TIDY "${CMAKE_CXX_CLANG_TIDY}"
            )
endif ()
