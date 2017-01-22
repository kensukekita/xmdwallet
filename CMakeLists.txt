cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    xmd-coin/external
    xmd-coin/include
    xmd-coin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("xmd-coin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  xmd-coin/external/miniupnpc/connecthostport.c
  xmd-coin/external/miniupnpc/igd_desc_parse.c
  xmd-coin/external/miniupnpc/minisoap.c
  xmd-coin/external/miniupnpc/miniupnpc.c
  xmd-coin/external/miniupnpc/miniwget.c
  xmd-coin/external/miniupnpc/minixml.c
  xmd-coin/external/miniupnpc/portlistingparse.c
  xmd-coin/external/miniupnpc/receivedata.c
  xmd-coin/external/miniupnpc/upnpcommands.c
  xmd-coin/external/miniupnpc/upnpreplyparse.c
  xmd-coin/src/Common/Base58.cpp
  xmd-coin/src/Common/CommandLine.cpp
  xmd-coin/src/Common/Util.cpp
  xmd-coin/src/Common/StringTools.cpp
  xmd-coin/src/Common/JsonValue.cpp
  xmd-coin/src/Common/ConsoleTools.cpp
  xmd-coin/src/Common/MemoryInputStream.cpp
  xmd-coin/src/Common/PathTools.cpp
  xmd-coin/src/Common/StdInputStream.cpp
  xmd-coin/src/Common/StdOutputStream.cpp
  xmd-coin/src/Common/StreamTools.cpp
  xmd-coin/src/Common/StringOutputStream.cpp
  xmd-coin/src/Common/StringView.cpp
  xmd-coin/src/Common/VectorOutputStream.cpp
  xmd-coin/src/crypto/blake256.c
  xmd-coin/src/crypto/chacha8.c
  xmd-coin/src/crypto/crypto-ops-data.c
  xmd-coin/src/crypto/crypto-ops.c
  xmd-coin/src/crypto/crypto.cpp
  xmd-coin/src/crypto/groestl.c
  xmd-coin/src/crypto/hash-extra-blake.c
  xmd-coin/src/crypto/hash-extra-groestl.c
  xmd-coin/src/crypto/hash-extra-jh.c
  xmd-coin/src/crypto/hash-extra-skein.c
  xmd-coin/src/crypto/hash.c
  xmd-coin/src/crypto/jh.c
  xmd-coin/src/crypto/keccak.c
  xmd-coin/src/crypto/oaes_lib.c
  xmd-coin/src/crypto/random.c
  xmd-coin/src/crypto/skein.c
  xmd-coin/src/crypto/slow-hash.c
  xmd-coin/src/crypto/slow-hash.cpp
  xmd-coin/src/crypto/tree-hash.c
  xmd-coin/src/CryptoNoteCore/BlockchainIndices.cpp
  xmd-coin/src/CryptoNoteCore/BlockchainMessages.cpp
  xmd-coin/src/CryptoNoteCore/BlockIndex.cpp
  xmd-coin/src/CryptoNoteCore/CoreConfig.cpp
  xmd-coin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  xmd-coin/src/CryptoNoteCore/CryptoNoteTools.cpp
  xmd-coin/src/CryptoNoteCore/Currency.cpp
  xmd-coin/src/CryptoNoteCore/MinerConfig.cpp
  xmd-coin/src/CryptoNoteCore/Transaction.cpp
  xmd-coin/src/CryptoNoteCore/Account.cpp
  xmd-coin/src/CryptoNoteCore/Blockchain.cpp
  xmd-coin/src/CryptoNoteCore/Checkpoints.cpp
  xmd-coin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  xmd-coin/src/CryptoNoteCore/Core.cpp
  xmd-coin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  xmd-coin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  xmd-coin/src/CryptoNoteCore/Difficulty.cpp
  xmd-coin/src/CryptoNoteCore/IBlock.cpp
  xmd-coin/src/CryptoNoteCore/Miner.cpp
  xmd-coin/src/CryptoNoteCore/TransactionExtra.cpp
  xmd-coin/src/CryptoNoteCore/TransactionPool.cpp
  xmd-coin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  xmd-coin/src/CryptoNoteCore/TransactionUtils.cpp
  xmd-coin/src/InProcessNode/InProcessNode.cpp
  xmd-coin/src/InProcessNode/InProcessNodeErrors.cpp
  xmd-coin/src/NodeRpcProxy/NodeErrors.cpp
  xmd-coin/src/NodeRpcProxy/NodeRpcProxy.cpp
  xmd-coin/src/P2p/NetNodeConfig.cpp
  xmd-coin/src/Serialization/BinaryInputStreamSerializer.cpp
  xmd-coin/src/Serialization/BinaryOutputStreamSerializer.cpp
  xmd-coin/src/Serialization/JsonInputValueSerializer.cpp
  xmd-coin/src/Serialization/JsonOutputStreamSerializer.cpp
  xmd-coin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  xmd-coin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  xmd-coin/src/Serialization/SerializationOverloads.cpp
  xmd-coin/src/Transfers/BlockchainSynchronizer.cpp
  xmd-coin/src/Transfers/SynchronizationState.cpp
  xmd-coin/src/Transfers/TransfersConsumer.cpp
  xmd-coin/src/Transfers/TransfersContainer.cpp
  xmd-coin/src/Transfers/TransfersSubscription.cpp
  xmd-coin/src/Transfers/TransfersSynchronizer.cpp
  xmd-coin/src/Wallet/LegacyKeysImporter.cpp
  xmd-coin/src/Wallet/WalletAsyncContextCounter.cpp
  xmd-coin/src/Wallet/WalletErrors.cpp
  xmd-coin/src/WalletLegacy/KeysStorage.cpp
  xmd-coin/src/WalletLegacy/WalletLegacy.cpp
  xmd-coin/src/WalletLegacy/WalletHelper.cpp
  xmd-coin/src/WalletLegacy/WalletLegacySerializer.cpp
  xmd-coin/src/WalletLegacy/WalletLegacySerialization.cpp
  xmd-coin/src/WalletLegacy/WalletTransactionSender.cpp
  xmd-coin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  xmd-coin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  xmd-coin/src/System/ContextGroup.cpp
  xmd-coin/src/System/Event.cpp
  xmd-coin/src/System/EventLock.cpp
  xmd-coin/src/System/InterruptedException.cpp
  xmd-coin/src/System/Ipv4Address.cpp
  xmd-coin/src/System/TcpStream.cpp
  xmd-coin/src/HTTP/HttpRequest.cpp
  xmd-coin/src/HTTP/HttpParser.cpp
  xmd-coin/src/HTTP/HttpParserErrorCodes.cpp
  xmd-coin/src/HTTP/HttpResponse.cpp
  xmd-coin/src/Rpc/HttpClient.cpp
  xmd-coin/src/Rpc/JsonRpc.cpp
  xmd-coin/src/P2p/NetNode.cpp
  xmd-coin/src/P2p/LevinProtocol.cpp
  xmd-coin/src/P2p/NetNodeConfig.cpp
  xmd-coin/src/P2p/PeerListManager.cpp
  xmd-coin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  xmd-coin/src/Logging/ILogger.cpp
  xmd-coin/src/Logging/LoggerGroup.cpp
  xmd-coin/src/Logging/CommonLogger.cpp
  xmd-coin/src/Logging/LoggerManager.cpp
  xmd-coin/src/Logging/FileLogger.cpp
  xmd-coin/src/Logging/StreamLogger.cpp
  xmd-coin/src/Logging/ConsoleLogger.cpp
  xmd-coin/src/Logging/LoggerMessage.cpp
  xmd-coin/src/Logging/LoggerRef.cpp
  xmd-coin/src/BlockchainExplorer/BlockchainExplorer.cpp
  xmd-coin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  xmd-coin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(xmd-coin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} xmd-coin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})

    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(xmd-coin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES xmd-coin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
