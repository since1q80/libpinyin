set(
    BINARY_MODEL_DATA
    gb_char.bin
    gbk_char.bin
    phrase_index.bin
    pinyin_index.bin
    bigram.db
)

set(
    BINARY_MODEL_DATA_FILES
    ${CMAKE_BINARY_DIR}/data/gb_char.bin
    ${CMAKE_BINARY_DIR}/data/gbk_char.bin
    ${CMAKE_BINARY_DIR}/data/phrase_index.bin
    ${CMAKE_BINARY_DIR}/data/pinyin_index.bin
    ${CMAKE_BINARY_DIR}/data/bigram.db
)

set(
    gen_binary_files_BIN
    ${CMAKE_BINARY_DIR}/utils/storage/gen_binary_files
)

set(
    import_interpolation_BIN
    ${CMAKE_BINARY_DIR}/utils/storage/import_interpolation
)

add_custom_target(
    data
    ALL
    DEPENDS
        ${BINARY_MODEL_DATA}
)

add_custom_command(
    OUTPUT
        gb_char.bin
        gbk_char.bin
        phrase_index.bin
        pinyin_index.bin
    COMMENT
        "Building binary model data..."
    COMMAND
        ${gen_binary_files_BIN} --table-dir ${CMAKE_SOURCE_DIR}/data
    DEPENDS
        gen_binary_files
)

add_custom_command(
    OUTPUT
        bigram.db
    COMMENT
        "Building binary bigram data..."
    COMMAND
        ${import_interpolation_BIN} < ${CMAKE_SOURCE_DIR}/data/interpolation.text
    DEPENDS
        import_interpolation
)

install(
    FILES
        ${BINARY_MODEL_DATA_FILES}
    DESTINATION
        ${DIR_SHARE_LIBPINYIN}/data
)

set_directory_properties(
    PROPERTIES
        ADDITIONAL_MAKE_CLEAN_FILES
            ${BINARY_MODEL_DATA_FILES}
)