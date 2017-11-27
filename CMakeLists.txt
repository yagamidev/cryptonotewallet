cmake_minimum_required(VERSION 2.8)

include(LipCoinWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(LipCoin_LIB LipCoin)

include_directories(${CMAKE_BINARY_DIR}
    src
    LipCoin/external
    LipCoin/include
    LipCoin/src)

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
configure_file("LipCoin/src/version.h.in" "version.h")
configure_file("src/LipCoinWalletConfig.h.in" "LipCoinWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(LipCoin_SOURCES
  LipCoin/external/miniupnpc/connecthostport.c
  LipCoin/external/miniupnpc/igd_desc_parse.c
  LipCoin/external/miniupnpc/minisoap.c
  LipCoin/external/miniupnpc/miniupnpc.c
  LipCoin/external/miniupnpc/miniwget.c
  LipCoin/external/miniupnpc/minixml.c
  LipCoin/external/miniupnpc/portlistingparse.c
  LipCoin/external/miniupnpc/receivedata.c
  LipCoin/external/miniupnpc/upnpcommands.c
  LipCoin/external/miniupnpc/upnpreplyparse.c
  LipCoin/src/Common/Base58.cpp
  LipCoin/src/Common/CommandLine.cpp
  LipCoin/src/Common/Util.cpp
  LipCoin/src/Common/StringTools.cpp
  LipCoin/src/Common/JsonValue.cpp
  LipCoin/src/Common/ConsoleTools.cpp
  LipCoin/src/Common/MemoryInputStream.cpp
  LipCoin/src/Common/PathTools.cpp
  LipCoin/src/Common/StdInputStream.cpp
  LipCoin/src/Common/StdOutputStream.cpp
  LipCoin/src/Common/StreamTools.cpp
  LipCoin/src/Common/StringOutputStream.cpp
  LipCoin/src/Common/StringView.cpp
  LipCoin/src/Common/VectorOutputStream.cpp
  LipCoin/src/crypto/blake256.c
  LipCoin/src/crypto/chacha8.c
  LipCoin/src/crypto/crypto-ops-data.c
  LipCoin/src/crypto/crypto-ops.c
  LipCoin/src/crypto/crypto.cpp
  LipCoin/src/crypto/groestl.c
  LipCoin/src/crypto/hash-extra-blake.c
  LipCoin/src/crypto/hash-extra-groestl.c
  LipCoin/src/crypto/hash-extra-jh.c
  LipCoin/src/crypto/hash-extra-skein.c
  LipCoin/src/crypto/hash.c
  LipCoin/src/crypto/jh.c
  LipCoin/src/crypto/keccak.c
  LipCoin/src/crypto/oaes_lib.c
  LipCoin/src/crypto/random.c
  LipCoin/src/crypto/skein.c
  LipCoin/src/crypto/slow-hash.c
  LipCoin/src/crypto/slow-hash.cpp
  LipCoin/src/crypto/tree-hash.c
  LipCoin/src/CryptoNoteCore/BlockchainIndices.cpp
  LipCoin/src/CryptoNoteCore/BlockchainMessages.cpp
  LipCoin/src/CryptoNoteCore/BlockIndex.cpp
  LipCoin/src/CryptoNoteCore/CoreConfig.cpp
  LipCoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  LipCoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  LipCoin/src/CryptoNoteCore/Currency.cpp
  LipCoin/src/CryptoNoteCore/MinerConfig.cpp
  LipCoin/src/CryptoNoteCore/Transaction.cpp
  LipCoin/src/CryptoNoteCore/Account.cpp
  LipCoin/src/CryptoNoteCore/Blockchain.cpp
  LipCoin/src/CryptoNoteCore/Checkpoints.cpp
  LipCoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  LipCoin/src/CryptoNoteCore/Core.cpp
  LipCoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  LipCoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  LipCoin/src/CryptoNoteCore/Difficulty.cpp
  LipCoin/src/CryptoNoteCore/IBlock.cpp
  LipCoin/src/CryptoNoteCore/Miner.cpp
  LipCoin/src/CryptoNoteCore/TransactionExtra.cpp
  LipCoin/src/CryptoNoteCore/TransactionPool.cpp
  LipCoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  LipCoin/src/CryptoNoteCore/TransactionUtils.cpp
  LipCoin/src/InProcessNode/InProcessNode.cpp
  LipCoin/src/InProcessNode/InProcessNodeErrors.cpp
  LipCoin/src/NodeRpcProxy/NodeErrors.cpp
  LipCoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  LipCoin/src/P2p/NetNodeConfig.cpp
  LipCoin/src/Serialization/BinaryInputStreamSerializer.cpp
  LipCoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  LipCoin/src/Serialization/JsonInputValueSerializer.cpp
  LipCoin/src/Serialization/JsonOutputStreamSerializer.cpp
  LipCoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  LipCoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  LipCoin/src/Serialization/SerializationOverloads.cpp
  LipCoin/src/Transfers/BlockchainSynchronizer.cpp
  LipCoin/src/Transfers/SynchronizationState.cpp
  LipCoin/src/Transfers/TransfersConsumer.cpp
  LipCoin/src/Transfers/TransfersContainer.cpp
  LipCoin/src/Transfers/TransfersSubscription.cpp
  LipCoin/src/Transfers/TransfersSynchronizer.cpp
  LipCoin/src/Wallet/LegacyKeysImporter.cpp
  LipCoin/src/Wallet/WalletAsyncContextCounter.cpp
  LipCoin/src/Wallet/WalletErrors.cpp
  LipCoin/src/WalletLegacy/KeysStorage.cpp
  LipCoin/src/WalletLegacy/WalletLegacy.cpp
  LipCoin/src/WalletLegacy/WalletHelper.cpp
  LipCoin/src/WalletLegacy/WalletLegacySerializer.cpp
  LipCoin/src/WalletLegacy/WalletLegacySerialization.cpp
  LipCoin/src/WalletLegacy/WalletTransactionSender.cpp
  LipCoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  LipCoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  LipCoin/src/System/ContextGroup.cpp
  LipCoin/src/System/Event.cpp
  LipCoin/src/System/EventLock.cpp
  LipCoin/src/System/InterruptedException.cpp
  LipCoin/src/System/Ipv4Address.cpp
  LipCoin/src/System/TcpStream.cpp
  LipCoin/src/HTTP/HttpRequest.cpp
  LipCoin/src/HTTP/HttpParser.cpp
  LipCoin/src/HTTP/HttpParserErrorCodes.cpp
  LipCoin/src/HTTP/HttpResponse.cpp
  LipCoin/src/Rpc/HttpClient.cpp
  LipCoin/src/Rpc/JsonRpc.cpp
  LipCoin/src/P2p/NetNode.cpp
  LipCoin/src/P2p/LevinProtocol.cpp
  LipCoin/src/P2p/NetNodeConfig.cpp
  LipCoin/src/P2p/PeerListManager.cpp
  LipCoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  LipCoin/src/Logging/ILogger.cpp
  LipCoin/src/Logging/LoggerGroup.cpp
  LipCoin/src/Logging/CommonLogger.cpp
  LipCoin/src/Logging/LoggerManager.cpp
  LipCoin/src/Logging/FileLogger.cpp
  LipCoin/src/Logging/StreamLogger.cpp
  LipCoin/src/Logging/ConsoleLogger.cpp
  LipCoin/src/Logging/LoggerMessage.cpp
  LipCoin/src/Logging/LoggerRef.cpp
  LipCoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  LipCoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  LipCoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(LipCoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/LipCoinwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(LipCoin_SOURCES ${LipCoin_SOURCES} LipCoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "LipCoin GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME LipCoinWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE LipCoin.icns)
    set(APPLICATION_ICON src/images/LipCoin.icns)
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

include_directories(LipCoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES LipCoin/src/Platform/${PLATFORM_DIR}/System/*)
set(LipCoin_SOURCES ${LipCoin_SOURCES} ${PLATFORM_SOURCES})

add_library(${LipCoin_LIB} STATIC ${LipCoin_SOURCES})
set_target_properties(${LipCoin_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${LipCoin_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${LipCoin_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
