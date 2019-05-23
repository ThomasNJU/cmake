cmake_minimum_required(VERSION 3.13)
project(project)

set(CMAKE_CXX_STANDARD 11)

MACRO(GET_ALL ROOT LEVEL ALL_SOURCE ALL_HEADER ALL_PATH)
#    message("START: CURDIR=" ${CURDIR} " LEVEL=" ${LEVEL})
    LIST(APPEND ${ALL_PATH} ${ROOT})
    message("PATH " ${ROOT})
    IF(LV1 MATCHES "tafjcedepend")
        MESSAGE("skip tafjcedepend$:" ${LV1})
   ELSEIF(${LEVEL} VERSION_GREATER 0)
        #        message("LEVEL > 0, LEVEL=" ${LEVEL})
        FILE(GLOB ALL_LV1  ${ROOT}/*)
        FOREACH(LV1 ${ALL_LV1})
#            MESSAGE("NEXT_LEVEL" ${LV1})
            IF(LV1 MATCHES "\\.svn$")
#                MESSAGE("skip svn:" ${LV1})
            ELSEIF(LV1 MATCHES "cmake-build-debug")
#                MESSAGE("skip tafjcedepend$:" ${LV1})
            ELSEIF(IS_DIRECTORY ${LV1})
#                MESSAGE("DIRECTORY:" ${LV1})
#                LIST(APPEND ${ALL_PATH} ${LV1})
                MATH(EXPR DEC_LEVEL "${LEVEL}-1")
                GET_ALL(${LV1} ${DEC_LEVEL} ${ALL_SOURCE} ${ALL_HEADER} ${ALL_PATH})
            ELSEIF(LV1 MATCHES "\\.c")
#                MESSAGE("SOURCE FILE:" ${LV1})
                LIST(APPEND ${ALL_SOURCE} ${LV1})
            ELSEIF(LV1 MATCHES "\\.h")
#                MESSAGE("HEADER FILE:" ${LV1})
                LIST(APPEND ${ALL_HEADER} ${LV1})
            ENDIF()
        ENDFOREACH()
    ELSE()
        #        message("LEVEL <= 0, LEVEL=" ${LEVEL})
    ENDIF()
ENDMACRO()

MACRO(ADD_SERVICES NAME PATH)
    SET(ALL_SOURCE)
    SET(ALL_HEADER)
    SET(ALL_PATH)
    GET_ALL(${PATH} 6 ALL_SOURCE ALL_HEADER ALL_PATH)

    add_executable(${NAME} ${ALL_SOURCE} ${ALL_HEADER})

    FOREACH(ONE_SOURCE ${ALL_SOURCE})
        MESSAGE(${NAME} " SOURCE " ${ONE_SOURCE})
    ENDFOREACH()

    FOREACH(ONE_SOURCE ${ALL_HEADER})
        MESSAGE(${NAME} " HEADER " ${ONE_SOURCE})
    ENDFOREACH()


    FOREACH(ONE_PATH ${ALL_PATH})
        target_include_directories(${NAME} PRIVATE ${ONE_PATH})
        MESSAGE(${NAME} " PRIVATE " ${ONE_PATH})
    ENDFOREACH()

#    include_directories(${ALL_PATH})


ENDMACRO()



#GET_ALL(secureserver/open_proj 4 source)
#GET_ALL(pimserver_svn/ 4 pimserver)

include_directories(Taf_V3Beta02_Build002_B010/)
include_directories(Taf_V3Beta02_Build002_B010/include)
include_directories(Taf_V3Beta02_Build002_B010/include/jce)
include_directories(Taf_V3Beta02_Build002_B010/include/util)

#include_directories(tafjcedepend)
#include_directories(rapidjson)
#include_directories(faiss)

#ADD_SERVICES(taf Taf_V3Beta02_Build002_B010/)
ADD_SERVICES(RcmdDataSvr Recommend/RcmdDataSvr/trunk)
ADD_SERVICES(DataDistributeSvr Recommend/DataDistributeSvr/trunk)
ADD_SERVICES(SwPlatformServer Recommend/SwPlatformServer/trunk)

#ADD_SERVICES(VerifyCodeSvr Recommend/VerifyCodeServer/)
#ADD_SERVICES(RTRcmdSvr Recommend/RTRcmdSvr/trunk)
#ADD_SERVICES(DataCenterSvr Recommend/DataCenterSvr/trunk)
ADD_SERVICES(RcmdSampleSvr Recommend/RcmdSampleSvr/trunk)
#ADD_SERVICES(RcmdTrainSvr Recommend/RcmdTrainSvr/trunk)
#ADD_SERVICES(RcmdPredictSvr Recommend/RcmdPredictSvr/trunk)
#ADD_SERVICES(faiss faiss)
#ADD_SERVICES(FaissProxySvr Recommend/FaissProxySvr/trunk)
#ADD_SERVICES(FaissWorkerSvr Recommend/FaissWorkerSvr/trunk)
#ADD_SERVICES(RJLogicServer Recommend/RJLogicServer/trunk)
#ADD_SERVICES(PhoneAttr Recommend/PhoneAttriServer)
#ADD_SERVICES(IOSAppBackupSvr secureserver/open_proj/IOSAppBackupSvr)


#ADD_SERVICES(CommonChannelServer secureserver/open_proj/CommonChannelServer)

#ADD_SERVICES(V2SessionServer pimserver_svn/V2SessionServer)
#ADD_SERVICES(VerifyCodeServer pimserver_svn/VerifyCodeServer)
#ADD_SERVICES(WupSyncServer pimserver_svn/WupSyncServer)
#ADD_SERVICES(GMAuthSvr pimserver_svn/GMAuthServer)
#ADD_SERVICES(MobileBindQQServer pimserver_svn/MobileBindQQServer)
