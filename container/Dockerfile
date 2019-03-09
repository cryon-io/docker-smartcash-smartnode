#  SMARTCASH Smartnode docker template
#  Copyright © 2019 cryon.io
#
#  This program is free software: you can redistribute it and/or modify
#  it under the terms of the GNU Affero General Public License as published
#  by the Free Software Foundation, either version 3 of the License, or
#  (at your option) any later version.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU Affero General Public License for more details.
#
#  You should have received a copy of the GNU Affero General Public License
#  along with this program.  If not, see <https://www.gnu.org/licenses/>.
#
#  Contact: cryi@tutanota.com

FROM ubuntu:bionic

# setup dependencies
RUN apt-get update && apt-get upgrade -q -y
RUN apt-get install wget curl libjemalloc1 unzip jq -y
RUN apt-get install software-properties-common -y
RUN add-apt-repository ppa:smartcash/ppa -y && apt update -y && apt install smartcashd -y

ADD healthcheck.sh /healthcheck.sh
RUN chmod +x /healthcheck.sh
HEALTHCHECK --interval=10s --timeout=30s --start-period=30s --retries=3 \
  CMD /healthcheck.sh

# create user
RUN adduser --disabled-login \
         --disabled-password \
         --gecos "" \
         --uid 15000 \
         smartcash

# copy necessary scripts
WORKDIR /home/smartcash/
COPY get-node-info.sh ./get-node-info.sh
COPY get-version.sh ./get-version.sh

# set execution rights
RUN chmod +x ./get-node-info.sh ./get-version.sh

# run smartcash masternode
USER smartcash
ENTRYPOINT [ "smartcashd" ] 
