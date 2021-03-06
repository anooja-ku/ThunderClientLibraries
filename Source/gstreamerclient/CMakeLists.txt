# If not stated otherwise in this file or this component's license file the
# following copyright and licenses apply:
#
# Copyright 2020 RDK Management
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

set(TARGET gstreamerclient)

option(GSTREAMER_CLIENT_IMPLEMENTATION "Defines which implementation is used." "None")

message("Setup ${TARGET} v${VERSION}...")

if (NOT "${GSTREAMER_CLIENT_IMPLEMENTATION}" STREQUAL "None")

	message("GstreamerClient impl. for ${GSTREAMER_CLIENT_IMPLEMENTATION}")
	find_package(GSTREAMER REQUIRED)
	find_package(GSTREAMER_BASE REQUIRED)

    if("${GSTREAMER_CLIENT_IMPLEMENTATION}" STREQUAL "Nexus")
        if (GSTREAMERCLIENT_IMPLEMENTATION_PATH)
            add_subdirectory(${GSTREAMERCLIENT_IMPLEMENTATION_PATH})
        else()
            include(GetExternalCode)
            set(GSTREAMERCLIENT_IMPLEMENTATION_VERSION "master" CACHE STRING "gstreamerclient implementation version")
            set(GSTREAMERCLIENT_IMPLEMENTATION_REPOSITORY "https://code.rdkcentral.com/r/soc/broadcom/components/rdkcentral/thunder/gstreamerclient" CACHE STRING "gstreamerclient implementation repository")
            GetExternalCode(
                GIT_REPOSITORY ${GSTREAMERCLIENT_IMPLEMENTATION_REPOSITORY}
                GIT_TAG ${GSTREAMERCLIENT_IMPLEMENTATION_VERSION}
                SOURCE_DIR "Nexus"
            )
            add_subdirectory(Nexus)
        endif()
    else()
        add_subdirectory (${GSTREAMER_CLIENT_IMPLEMENTATION})
    endif()
endif()

install(
	FILES gstreamerclient.h
        DESTINATION include/${NAMESPACE}/gstreamerclient/
)
