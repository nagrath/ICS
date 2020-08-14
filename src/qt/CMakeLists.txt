CMAKE_MINIMUM_REQUIRED(VERSION 3.10)

set(QT_MINIMUM_VERSION 5.5.1)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOUIC_SEARCH_PATHS ${CMAKE_CURRENT_SOURCE_DIR}/forms ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/forms ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/forms)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
endif ()

unset(Qt5Enables)
foreach(comp Widgets Core Gui Network LinguistTools DBus Svg Charts)
    find_package(Qt5${comp})
    set(Qt5Enables "${Qt5Enables};Qt5${comp}_FOUND")
    if(Qt5${comp}_FOUND)
        MESSAGE(STATUS "FOUND Qt5${comp}")
        include_directories(${include_directories} ${Qt5${comp}_INCLUDE_DIRS} ${QT_USE_FILE})
        add_definitions(${Qt5${comp}_DEFINITIONS})
        list(APPEND Qt5_LIBRARIES ${Qt5${comp}_LIBRARIES})
    else()
        MESSAGE(WARNING "Cant find Qt5${comp}")
    endif()
endforeach(comp)

if (Qt5Core_FOUND)
    if (Qt5Core_VERSION VERSION_LESS QT_MINIMUM_VERSION)
        MESSAGE(FATAL_ERROR "Minimum supported Qt5 version is ${QT_MINIMUM_VERSION}")
    else()
        MESSAGE(STATUS "Found Qt5 version: ${Qt5Core_VERSION}")
    endif()
else()
    MESSAGE(FATAL_ERROR "Could not find any Qt5 installation")
endif()

if (Qt5DBus_FOUND)
    add_compile_options("-DUSE_DBUS")
endif()

if (Qt5Charts_FOUND)
    add_compile_options("-DUSE_QTCHARTS")
endif()

find_package(Qrcode)
if (QRCODE_FOUND)
    include_directories ( ${QRCODE_INCLUDE_DIR} )
    link_directories ( ${QRCODE_LIBRARY_DIRS} )
endif()

find_program(MOC_BIN NAMES moc moc-qt5 qt5-moc
        PATHS /usr/lib/qt5/bin /usr/lib/x86_64-linux-gnu/qt5/bin /usr/local/opt/qt/bin /usr/local/opt/qt5/bin
        NO_DEFAULT_PATH
        NO_SYSTEM_ENVIRONMENT_PATH)
if (MOC_BIN)
    MESSAGE(STATUS "FOUND MOC ${MOC_BIN}")
endif ()

add_compile_options("-DQT_NO_KEYWORDS")

# Why isn't this done automatically??
if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(CMAKE_AUTOMOC_MOC_OPTIONS "-DQ_OS_MAC")
endif()

file(GLOB LOCAL_QT_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/*.h)
source_group("QtHeaders" FILES ${LOCAL_QT_HEADERS})

find_package(Protobuf REQUIRED)
if(Protobuf_FOUND)
    message(STATUS "Found Protobuf compiler: ${Protobuf_PROTOC_EXECUTABLE}")
    message(STATUS "Found Protobuf library: ${Protobuf_LIBRARIES}")
endif()
include_directories(${PROTOBUF_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_BINARY_DIR})

PROTOBUF_GENERATE_CPP(PROTO_SRCS PROTO_HDRS ${CMAKE_CURRENT_SOURCE_DIR}/paymentrequest.proto)

SET(QT_SOURCES
        ${CMAKE_CURRENT_SOURCE_DIR}/bantablemodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/bitcoinaddressvalidator.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/bitcoinunits.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/clientmodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/csvmodelwriter.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/guiutil.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/intro.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/networkstyle.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/notificator.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/optionsdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/optionsmodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/peertablemodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/platformstyle.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/qvalidatedlineedit.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/qvaluecombobox.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/rpcconsole.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/trafficgraphwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/utilitydialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/addressbookpage.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/addresstablemodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/askpassphrasedialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/coincontroldialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/coincontroltreewidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/editaddressdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/openuridialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/paymentrequestplus.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/paymentserver.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/recentrequeststablemodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/transactiondesc.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/transactionfilterproxy.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/transactionrecord.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/transactiontablemodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/walletmodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/walletmodeltransaction.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/iqcashgui.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/loadingdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/pwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/focuseddialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/guitransactionsutils.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/snackbar.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/navmenuwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/lockunlock.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/receivedialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/furabstractlistitemdelegate.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/txviewholder.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/qtutils.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/expandablebutton.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/topbar.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/txrow.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/addressfilterproxymodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/dashboardwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/addresslabelrow.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/contactdropdownrow.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/contactsdropdown.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/optionbutton.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/mninfodialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/coldstakingmodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/mnmodel.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/masternodewizarddialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/mnrow.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/masternodeswidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/sendchangeaddressdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/sendconfirmdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/sendcustomfeedialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/sendchangeaddressdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/sendmultirow.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/send.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/addnewaddressdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/addnewcontactdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/requestdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/receivewidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/myaddressrow.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/csrow.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/addressholder.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/tooltipmenu.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/addresseswidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/defaultdialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/coldstakingwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsbackupwallet.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsbittoolwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsconsolewidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsdisplayoptionswidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsfaqwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsinformationwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsmainoptionswidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsmultisenddialog.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsmultisendwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingssignmessagewidgets.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingswalletoptionswidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingswalletrepairwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsexportcsv.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingswidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/welcomecontentwidget.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/splash.cpp
        )

# Workaround until the old rpcconsole UI window is fully removed
set_property(SOURCE ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings/settingsconsolewidget.cpp PROPERTY SKIP_AUTOMOC ON)
execute_process(
        COMMAND ${MOC_BIN} -o moc_settingsconsolewidget.cpp settingsconsolewidget.h
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings
)

execute_process(
        COMMAND ${MOC_BIN} -o moc_pfborderimage.cpp pfborderimage.h
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/iqcash
)
list(APPEND QT_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/moc_pfborderimage.cpp)

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    list(APPEND QT_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/macdockiconhandler.mm)
    list(APPEND QT_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/macnotificationhandler.mm)
endif()

add_library(qt_stuff STATIC ${BitcoinHeaders} ${QtHeaders} ${QT_SOURCES} ${PROTO_SRCS} ${PROTO_HDRS})
target_include_directories(qt_stuff PUBLIC ${CMAKE_SOURCE_DIR}/src
        ${CMAKE_SOURCE_DIR}/src/leveldb/include
        ${CMAKE_SOURCE_DIR}/src/univalue/include
        ${CMAKE_SOURCE_DIR}/src/secp256k1/include
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash
        ${CMAKE_CURRENT_SOURCE_DIR}/iqcash/settings
        ${OPENSSL_INCLUDE_DIR}
        ${BerkeleyDB_INCLUDE_DIRS}
        )
set_property(TARGET qt_stuff PROPERTY CXX_STANDARD 11)

file(GLOB QT_TRANSLATIONS_FILES ${CMAKE_CURRENT_SOURCE_DIR}/locale/*.ts)

get_filename_component(QM_FILES_BINARY_DIR "${CMAKE_CURRENT_SOURCE_DIR}/locale" ABSOLUTE)
set_source_files_properties(${QT_TRANSLATIONS_FILES} PROPERTIES OUTPUT_LOCATION "${QM_FILES_BINARY_DIR}")

QT5_ADD_TRANSLATION(QM ${QT_TRANSLATIONS_FILES})
add_custom_target(translations_target ALL DEPENDS ${QM})

QT5_ADD_RESOURCES(QRC_RESOURCE iqcash.qrc)
QT5_ADD_RESOURCES(QRC_LOCALE_RESOURCE iqcash_locale.qrc)

add_executable(iqcash-qt iqcash.cpp ${QM} ${QRC_RESOURCE} ${QRC_LOCALE_RESOURCE})
add_dependencies(iqcash-qt translations_target libunivalue libsecp256k1 libzcashrust leveldb crc32c)
target_include_directories(iqcash-qt PUBLIC ${ENDIAN_INCLUDES})
target_link_libraries(iqcash-qt
        qt_stuff
        univalue
        SERVER_A UTIL_A WALLET_A CLI_A COMMON_A BITCOIN_CRYPTO_A ZEROCOIN_A SAPLING_A
        leveldb crc32c secp256k1 rustzcash
        ${BerkeleyDB_LIBRARIES} ${OPENSSL_LIBRARIES} ${Boost_LIBRARIES} ${PROTOBUF_LIBRARIES} ${LIBEVENT_LIB}
        ${sodium_LIBRARY_RELEASE}
        -ldl pthread
        )
if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    target_link_libraries(iqcash-qt "-framework Cocoa")
endif()
if(GMP_FOUND)
    target_link_libraries(iqcash-qt ${GMP_LIBRARY})
    target_include_directories(iqcash-qt PUBLIC ${GMP_INCLUDE_DIR})
endif()
if(ZMQ_FOUND)
    target_link_libraries(iqcash-qt ZMQ_A ${ZMQ_LIB})
    target_include_directories(iqcash-qt PUBLIC ${ZMQ_INCLUDE_DIR})
endif()
if (QRCODE_FOUND)
    target_link_libraries(iqcash-qt ${QRCODE_LIB})
    target_include_directories(iqcash-qt PUBLIC ${QRCODE_INCLUDE_DIR})
endif()
if(MINIUPNP_FOUND)
    target_compile_definitions(iqcash-qt PUBLIC "-DSTATICLIB -DMINIUPNP_STATICLIB")
    target_link_libraries(iqcash-qt ${MINIUPNP_LIBRARY})
    target_include_directories(iqcash-qt PUBLIC ${MINIUPNP_INCLUDE_DIR})
endif()

target_link_libraries(iqcash-qt Qt5::Gui Qt5::Core Qt5::Widgets Qt5::Network Qt5::Svg ${QT_LIBRARIES})
if (Qt5DBus_FOUND)
    target_link_libraries(iqcash-qt Qt5::DBus ${QT_LIBRARIES})
endif()
if (Qt5Charts_FOUND)
    target_link_libraries(iqcash-qt Qt5::Charts ${QT_LIBRARIES})
endif()
