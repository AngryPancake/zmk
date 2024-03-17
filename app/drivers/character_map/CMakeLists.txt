# Copyright (c) 2023 The ZMK Contributors
# SPDX-License-Identifier: MIT

zephyr_library_named(zmk__drivers__character_map)
zephyr_library_include_directories(${CMAKE_SOURCE_DIR}/include/)

zephyr_library_sources_ifdef(CONFIG_ZMK_CHARACTER_MAP character_map.c)
