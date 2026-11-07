# Zephyr 专用的 NanoPb集成.
if(CONFIG_NANOPB)
  list(APPEND CMAKE_MODULE_PATH "${ZEPHYR_BASE}/modules/nanopb")
  include(nanopb)

  zephyr_library_named(rp_proto)
  get_filename_component(RP_PROTO_DIR "${CMAKE_CURRENT_LIST_DIR}/../proto" ABSOLUTE)
  set(NANOPB_IMPORT_DIRS "${RP_PROTO_DIR}")
  file(GLOB_RECURSE PROTO_SOURCES CONFIGURE_DEPENDS "${RP_PROTO_DIR}/*.proto")
  file(GLOB_RECURSE PROTO_OPTIONS CONFIGURE_DEPENDS "${RP_PROTO_DIR}/*.options")

  list(APPEND NANOPB_DEPENDS ${PROTO_SOURCES} ${PROTO_OPTIONS})
  set(NANOPB_GENERATE_CPP_STANDALONE OFF)
  set(NANOPB_GENERATE_CPP_APPEND_PATH OFF)

  nanopb_generate_cpp(PROTO_SRCS PROTO_HDRS RELPATH "${RP_PROTO_DIR}" ${PROTO_SOURCES})
  target_sources(rp_proto PRIVATE ${PROTO_SRCS} ${PROTO_HDRS})
  target_include_directories(rp_proto PUBLIC "${CMAKE_CURRENT_BINARY_DIR}")

  add_custom_target(rp_proto_generated_headers DEPENDS ${PROTO_HDRS})
  add_dependencies(nanopb_generated_headers rp_proto_generated_headers)
  add_dependencies(rp_proto nanopb_generated_headers)
endif()
