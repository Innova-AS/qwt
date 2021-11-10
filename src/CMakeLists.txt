set(QT_VER Qt5)
set(QT_COMPONENTS
  Concurrent
  Core
  PrintSupport
  Widgets
)

add_compile_definitions(QWT_MOC_INCLUDE=1)

if(NOT ENABLE_SVG)
  add_compile_definitions(QWT_NO_SVG)
endif()

if(NOT ENABLE_OPENGL)
  add_compile_definitions(QWT_NO_OPENGL)
endif()

if(NOT ENABLE_DLL)
  add_compile_definitions(QWT_NO_DLL)
endif()

if(ENABLE_OPENGL)
  message(STATUS "ADDING OPENGL COMPONENT")
  list(APPEND QT_COMPONENTS OpenGL)
endif()

if(ENABLE_SVG)
  message(STATUS "ADDING SVG COMPONENT")
  list(APPEND QT_COMPONENTS Svg)
endif()

foreach(_C IN LISTS QT_COMPONENTS)
  list(APPEND QT_LIBS ${QT_VER}::${_C})
endforeach()

find_package(${QT_VER}
  COMPONENTS
    ${QT_COMPONENTS}
  REQUIRED
)

add_library(_spline OBJECT
  qwt_spline.cpp
  qwt_spline_basis.cpp
  qwt_spline_cubic.cpp
  qwt_spline_local.cpp
  qwt_spline_parametrization.cpp
  qwt_spline_pleasing.cpp
  qwt_spline_polynomial.cpp
)
target_link_libraries(_spline PRIVATE ${QT_LIBS})

add_library(_scale OBJECT
  qwt_scale_div.cpp
  qwt_scale_draw.cpp
  qwt_scale_engine.cpp
  qwt_scale_map.cpp
)
target_link_libraries(_scale PRIVATE ${QT_LIBS})

add_library(_date OBJECT
  qwt_date.cpp
  qwt_date_scale_draw.cpp
  qwt_date_scale_engine.cpp
)
target_link_libraries(_date PRIVATE ${QT_LIBS})

add_library(_text OBJECT
  qwt_text.cpp
  qwt_text_engine.cpp
  qwt_text_label.cpp
)
target_link_libraries(_text PRIVATE ${QT_LIBS})

add_library(_base OBJECT
  qwt_abstract_scale_draw.cpp
  qwt_bezier.cpp
  qwt_clipper.cpp
  qwt_color_map.cpp
  qwt_column_symbol.cpp
  qwt_dyngrid_layout.cpp
  qwt_event_pattern.cpp
  qwt_graphic.cpp
  qwt_interval.cpp
  qwt_interval_symbol.cpp
  qwt_magnifier.cpp
  qwt_math.cpp
  qwt_null_paintdevice.cpp
  qwt_painter.cpp
  qwt_painter_command.cpp
  qwt_panner.cpp
  qwt_picker.cpp
  qwt_picker_machine.cpp
  qwt_pixel_matrix.cpp
  qwt_point_3d.cpp
  qwt_point_mapper.cpp
  qwt_round_scale_draw.cpp
  qwt_symbol.cpp
  qwt_system_clock.cpp
  qwt_transform.cpp
  qwt_widget_overlay.cpp
)
target_link_libraries(_base PRIVATE ${QT_LIBS})

if(ENABLE_WIDGETS)
  add_library(_widgets OBJECT
    qwt_abstract_scale.cpp
    qwt_abstract_slider.cpp
    qwt_analog_clock.cpp
    qwt_arrow_button.cpp
    qwt_compass.cpp
    qwt_compass_rose.cpp
    qwt_counter.cpp
    qwt_dial.cpp
    qwt_dial_needle.cpp
    qwt_knob.cpp
    qwt_slider.cpp
    qwt_thermo.cpp
    qwt_wheel.cpp
  )
  target_link_libraries(_widgets PRIVATE ${QT_LIBS})
  list(APPEND OPT_LIBS _widgets)
endif()

if(ENABLE_PLOT)
  add_library(_plot OBJECT
    qwt_abstract_legend.cpp
    qwt_curve_fitter.cpp
    qwt_legend.cpp
    qwt_legend_data.cpp
    qwt_legend_label.cpp
    qwt_matrix_raster_data.cpp
    qwt_plot.cpp
    qwt_plot_abstract_barchart.cpp
    qwt_plot_abstract_canvas.cpp
    qwt_plot_axis.cpp
    qwt_plot_barchart.cpp
    qwt_plot_canvas.cpp
    qwt_plot_curve.cpp
    qwt_plot_dict.cpp
    qwt_plot_directpainter.cpp
    qwt_plot_graphicitem.cpp
    qwt_plot_grid.cpp
    qwt_plot_histogram.cpp
    qwt_plot_intervalcurve.cpp
    qwt_plot_item.cpp
    qwt_plot_layout.cpp
    qwt_plot_legenditem.cpp
    qwt_plot_magnifier.cpp
    qwt_plot_marker.cpp
    qwt_plot_multi_barchart.cpp
    qwt_plot_panner.cpp
    qwt_plot_picker.cpp
    qwt_plot_rasteritem.cpp
    qwt_plot_renderer.cpp
    qwt_plot_rescaler.cpp
    qwt_plot_scaleitem.cpp
    qwt_plot_seriesitem.cpp
    qwt_plot_shapeitem.cpp
    qwt_plot_spectrocurve.cpp
    qwt_plot_spectrogram.cpp
    qwt_plot_textlabel.cpp
    qwt_plot_tradingcurve.cpp
    qwt_plot_vectorfield.cpp
    qwt_plot_zoneitem.cpp
    qwt_plot_zoomer.cpp
    qwt_point_data.cpp
    qwt_raster_data.cpp
    qwt_sampling_thread.cpp
    qwt_scale_widget.cpp
    qwt_series_data.cpp
    qwt_spline_curve_fitter.cpp
    qwt_vectorfield_symbol.cpp
    qwt_weeding_curve_fitter.cpp
  )
  target_link_libraries(_plot PRIVATE ${QT_LIBS})
  list(APPEND OPT_LIBS _plot)

  if(ENABLE_OPENGL)
    add_library(_opengl OBJECT
      qwt_plot_glcanvas.cpp
      qwt_plot_opengl_canvas.cpp
    )
    target_link_libraries(_opengl PRIVATE ${QT_LIBS})
    list(APPEND OPT_LIBS _opengl)
  endif()

  if(ENABLE_SVG)
    add_library(_svg OBJECT qwt_plot_svgitem.cpp)
    target_link_libraries(_svg PRIVATE ${QT_LIBS})
    list(APPEND OPT_LIBS _svg)
  endif()

  if(ENABLE_POLAR)
    add_library(_polar OBJECT
      qwt_point_polar.cpp
      qwt_polar_canvas.cpp
      qwt_polar_curve.cpp
      qwt_polar_fitter.cpp
      qwt_polar_grid.cpp
      qwt_polar_item.cpp
      qwt_polar_itemdict.cpp
      qwt_polar_layout.cpp
      qwt_polar_magnifier.cpp
      qwt_polar_marker.cpp
      qwt_polar_panner.cpp
      qwt_polar_picker.cpp
      qwt_polar_plot.cpp
      qwt_polar_renderer.cpp
      qwt_polar_spectrogram.cpp
    )
    target_link_libraries(_polar PRIVATE ${QT_LIBS})
    list(APPEND OPT_LIBS _polar)
  endif()
endif()

add_library(qwt STATIC qwt.cpp)
target_link_libraries(qwt
  PUBLIC
    ${QT_LIBS}
  PRIVATE
    _base
    _date
    _scale
    _spline
    _text
    ${OPT_LIBS}
)
target_include_directories(qwt PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
