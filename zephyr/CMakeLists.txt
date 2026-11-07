# Zephyr 专用的 NanoPb集成.
if(CONFIG_NANOPB)
  list(APPEND CMAKE_MODULE_PATH "${ZEPHYR_BASE}/modules/nanopb")
  include(nanopb)

  zephyr_library_named(rp_proto)
  get_filename_component(RP_PROTO_DIR "${CMAKE_CURRENT_LIST_DIR}/../proto" ABSOLUTE)
  set(NANOPB_IMPORT_DIRS "${RP_PROTO_DIR}")
  file(GLOB_RECURSE PROTO_SOURCES "${CMAKE_CURRENT_LIST_DIR}/../proto/*.proto")
  zephyr_nanopb_sources(rp_proto ${PROTO_SOURCES})
endif()
