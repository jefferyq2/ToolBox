# Ported from libui/meson.build
add_library(ThirdParty_libui STATIC)
add_library(ThirdParty::libui ALIAS ThirdParty_libui)

set_target_properties(ThirdParty_libui PROPERTIES
  C_STANDARD 99                 # strict c99
  POSITION_INDEPENDENT_CODE ON  # use PIC even for static libraries
  CPP_STANDARD 11               # strict c++11
)

if(CMAKE_SYSTEM_NAME MATCHES "Darwin")
  enable_language(OBJC)
endif()  # darwin

if(MSVC)
  target_compile_options(ThirdParty_libui PUBLIC
    /wd4100
    /bigobj
  )
  if(CMAKE_BUILD_TYPE MATCHES "Debug" OR CMAKE_BUILD_TYPE MATCHES "RelWithDebInfo")
    target_compile_options(ThirdParty_libui PUBLIC
      /RTC1
      /RTCs
      /RTCu
    )
  endif()  # Debug
  target_link_options(ThirdParty_libui PUBLIC
    /LARGEADDRESSAWARE
    # /INCREMENTAL:NO
    /MANIFEST:NO
  )
else()  # not MSVC
  target_compile_options(ThirdParty_libui PUBLIC
		-Wno-unused-parameter
		-Wno-switch
  )
  if(CMAKE_SYSTEM_NAME MATCHES "Windows")
    target_link_options(ThirdParty_libui PUBLIC
      -static
      -static-libgcc
      -static-libstdc++
    )
  endif()  # Windows
endif()  # not MSVC

target_compile_definitions(ThirdParty_libui PUBLIC _UI_STATIC)

# common/meson.build
target_sources(ThirdParty_libui PRIVATE
  libui/common/attribute.c
  libui/common/attrlist.c
  libui/common/attrstr.c
  libui/common/areaevents.c
  libui/common/control.c
  libui/common/debug.c
  libui/common/matrix.c
  libui/common/opentype.c
  libui/common/shouldquit.c
  libui/common/tablemodel.c
  libui/common/tablevalue.c
  libui/common/userbugs.c
  libui/common/utf.c
)

if(CMAKE_SYSTEM_NAME MATCHES "Windows")
  # windows/meson.build
  target_sources(ThirdParty_libui PRIVATE
    libui/windows/alloc.cpp
    libui/windows/area.cpp
    libui/windows/areadraw.cpp
    libui/windows/areaevents.cpp
    libui/windows/areascroll.cpp
    libui/windows/areautil.cpp
    libui/windows/attrstr.cpp
    libui/windows/box.cpp
    libui/windows/button.cpp
    libui/windows/checkbox.cpp
    libui/windows/colorbutton.cpp
    libui/windows/colordialog.cpp
    libui/windows/combobox.cpp
    libui/windows/container.cpp
    libui/windows/control.cpp
    libui/windows/d2dscratch.cpp
    libui/windows/datetimepicker.cpp
    libui/windows/debug.cpp
    libui/windows/draw.cpp
    libui/windows/drawmatrix.cpp
    libui/windows/drawpath.cpp
    libui/windows/drawtext.cpp
    libui/windows/dwrite.cpp
    libui/windows/editablecombo.cpp
    libui/windows/entry.cpp
    libui/windows/events.cpp
    libui/windows/fontbutton.cpp
    libui/windows/fontdialog.cpp
    libui/windows/fontmatch.cpp
    libui/windows/form.cpp
    libui/windows/graphemes.cpp
    libui/windows/grid.cpp
    libui/windows/group.cpp
    libui/windows/image.cpp
    libui/windows/init.cpp
    libui/windows/label.cpp
    libui/windows/main.cpp
    libui/windows/menu.cpp
    libui/windows/multilineentry.cpp
    libui/windows/opentype.cpp
    libui/windows/parent.cpp
    libui/windows/progressbar.cpp
    libui/windows/radiobuttons.cpp
    libui/windows/separator.cpp
    libui/windows/sizing.cpp
    libui/windows/slider.cpp
    libui/windows/spinbox.cpp
    libui/windows/stddialogs.cpp
    libui/windows/tab.cpp
    libui/windows/table.cpp
    libui/windows/tabledispinfo.cpp
    libui/windows/tabledraw.cpp
    libui/windows/tableediting.cpp
    libui/windows/tablemetrics.cpp
    libui/windows/tabpage.cpp
    libui/windows/text.cpp
    libui/windows/utf16.cpp
    libui/windows/utilwin.cpp
    libui/windows/window.cpp
    libui/windows/winpublic.cpp
    libui/windows/winutil.cpp
    libui/ui_windows.h
  )
  target_link_libraries(ThirdParty_libui PUBLIC
    user32
    kernel32
    gdi32
    comctl32
    uxtheme
    msimg32
    comdlg32
    d2d1
    dwrite
    ole32
    oleaut32
    oleacc
    uuid
    windowscodecs
  )
  target_sources(ThirdParty_libui PUBLIC
    libui/windows/libui.manifest
    # libui/windows/resources.rc
  )
elseif(CMAKE_SYSTEM_NAME MATCHES "Darwin")
  # darwin/meson.build
  target_sources(ThirdParty_libui PRIVATE
	  libui/darwin/aat.m
	  libui/darwin/alloc.m
	  libui/darwin/area.m
	  libui/darwin/areaevents.m
	  libui/darwin/attrstr.m
	  libui/darwin/autolayout.m
	  libui/darwin/box.m
	  libui/darwin/button.m
	  libui/darwin/checkbox.m
	  libui/darwin/colorbutton.m
	  libui/darwin/combobox.m
	  libui/darwin/control.m
	  libui/darwin/datetimepicker.m
	  libui/darwin/debug.m
	  libui/darwin/draw.m
	  libui/darwin/drawtext.m
	  libui/darwin/editablecombo.m
	  libui/darwin/entry.m
	  libui/darwin/fontbutton.m
	  libui/darwin/fontmatch.m
	  libui/darwin/fonttraits.m
	  libui/darwin/fontvariation.m
	  libui/darwin/form.m
	  libui/darwin/future.m
	  libui/darwin/graphemes.m
	  libui/darwin/grid.m
	  libui/darwin/group.m
	  libui/darwin/image.m
	  libui/darwin/label.m
	  libui/darwin/main.m
	  libui/darwin/map.m
	  libui/darwin/menu.m
	  libui/darwin/multilineentry.m
	  libui/darwin/opentype.m
	  libui/darwin/progressbar.m
	  libui/darwin/radiobuttons.m
	  libui/darwin/scrollview.m
	  libui/darwin/separator.m
	  libui/darwin/slider.m
	  libui/darwin/spinbox.m
	  libui/darwin/stddialogs.m
	  libui/darwin/tab.m
	  libui/darwin/table.m
	  libui/darwin/tablecolumn.m
	  libui/darwin/text.m
	  libui/darwin/undocumented.m
	  libui/darwin/util.m
	  libui/darwin/window.m
	  libui/darwin/winmoveresize.m
    libui/ui_darwin.h
  )
  target_link_libraries(ThirdParty_libui PUBLIC Foundation AppKit)
  set_target_properties(ThirdParty_libui PROPERTIES SOVERSION A)
else()  # unix
  # unix/meson.build
  target_sources(ThirdParty_libui PRIVATE
	  libui/unix/alloc.c
	  libui/unix/area.c
	  libui/unix/attrstr.c
	  libui/unix/box.c
	  libui/unix/button.c
	  libui/unix/cellrendererbutton.c
	  libui/unix/checkbox.c
	  libui/unix/child.c
	  libui/unix/colorbutton.c
	  libui/unix/combobox.c
	  libui/unix/control.c
	  libui/unix/datetimepicker.c
	  libui/unix/debug.c
	  libui/unix/draw.c
	  libui/unix/drawmatrix.c
	  libui/unix/drawpath.c
	  libui/unix/drawtext.c
	  libui/unix/editablecombo.c
	  libui/unix/entry.c
	  libui/unix/fontbutton.c
	  libui/unix/fontmatch.c
	  libui/unix/form.c
	  libui/unix/future.c
	  libui/unix/graphemes.c
	  libui/unix/grid.c
	  libui/unix/group.c
	  libui/unix/image.c
	  libui/unix/label.c
	  libui/unix/main.c
	  libui/unix/menu.c
	  libui/unix/multilineentry.c
	  libui/unix/opentype.c
	  libui/unix/progressbar.c
	  libui/unix/radiobuttons.c
	  libui/unix/separator.c
	  libui/unix/slider.c
	  libui/unix/spinbox.c
	  libui/unix/stddialogs.c
	  libui/unix/tab.c
	  libui/unix/table.c
	  libui/unix/tablemodel.c
	  libui/unix/text.c
	  libui/unix/util.c
	  libui/unix/window.c
    libui/ui_unix.h
  )
  find_package(PkgConfig REQUIRED)
  pkg_check_modules(GTK gtk+-3.0 REQUIRED)
  target_link_libraries(ThirdParty_libui PUBLIC
    ${GTK_LIBRARIES}
    ${GTK_LINK_LIBRARIES}
  )
  # target_link_options(ThirdParty_libui PUBLIC
  #   ${GTK_LDFLAGS}
  #   ${GTK_LDFLAGS_OTHER}
  # )
  target_include_directories(ThirdParty_libui SYSTEM PUBLIC ${GTK_INCLUDE_DIRS})
  target_compile_options(ThirdParty_libui PUBLIC
    ${GTK_CFLAGS}
    ${GTK_CFLAGS_OTHER}
  )
  string(REGEX MATCH "([0-9]+)\\.([0-9]+)\\.([0-9]+)" GTK_VERSION ${GTK_VERSION})
  set(GTK_VERSION_MAJOR ${CMAKE_MATCH_1})
  set(GTK_VERSION_MINOR ${CMAKE_MATCH_2})
  set(GTK_VERSION_PATCH ${CMAKE_MATCH_3})
  if(GTK_VERSION_MAJOR LESS 3
     OR (GTK_VERSION_MAJOR EQUAL 3 AND GTK_VERSION_MINOR LESS 10))
    message(FATAL_ERROR "GTK+ version 3.10.0 or higher required")
  endif()  # GTK_VERSION
  target_link_libraries(ThirdParty_libui PUBLIC m dl)
  set_target_properties(ThirdParty_libui PROPERTIES SOVERSION 0)
endif()  # unix

target_sources(ThirdParty_libui PRIVATE
  libui/ui.h
)

target_include_directories(ThirdParty_libui SYSTEM PUBLIC
  ${CMAKE_CURRENT_LIST_DIR}
)

# Disable compile warning
target_compile_options(ThirdParty_libui PRIVATE
  $<$<CXX_COMPILER_ID:MSVC>:/w>
  $<$<OR:$<CXX_COMPILER_ID:GNU>,$<PLATFORM_ID:Linux>>:-w>
  $<$<OR:$<CXX_COMPILER_ID:GNU>,$<PLATFORM_ID:Linux>>:-Wno-everything>  # clang
)

find_package(Threads REQUIRED)
target_link_libraries(ThirdParty_libui PUBLIC Threads::Threads)
