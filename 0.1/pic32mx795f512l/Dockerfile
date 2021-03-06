FROM ubuntu:16.04
LABEL version="0.1"
MAINTAINER Pedro Calixto - pgcalixto

ARG MPLABX_VERSION=4.15
ARG XC32_VERSION=2.05

# Sets up environment for MPLAB tools installation
RUN dpkg --add-architecture i386 \
 \
 && apt-get update && apt-get install -qq \
    libc6:i386 \
    lld-5.0 \
    lldb-5.0 \
    libexpat1:i386 \
    libgtk2.0-0 \
    libssl-dev \
    libstdc++6:i386 \
    libx11-6:i386 \
    libxext6:i386 \
    libxext6 \
    libxrender1 \
    libxslt1.1 \
    libxtst6 \
    wget \
 \
 # Download and install xc32 compiler
 && wget -O /tmp/xc32.run "http://ww1.microchip.com/downloads/en/DeviceDoc/xc32-v${XC32_VERSION}-full-install-linux-installer.run" \
 && chmod a+x /tmp/xc32.run \
 && /tmp/xc32.run --mode unattended --unattendedmodeui none \
    --netservername localhost --LicenseType FreeMode \
 && rm /tmp/xc32.run \
 \
 # Download and install MPLAB X IDE
 && wget -O /tmp/mplabx-installer.tar "http://ww1.microchip.com/downloads/en/DeviceDoc/MPLABX-v${MPLABX_VERSION}-linux-installer.tar" \
 && tar -xvf /tmp/mplabx-installer.tar && rm /tmp/mplabx-installer.tar \
 && USER=root ./MPLABX-v${MPLABX_VERSION}-linux-installer.sh --nox11 \
    -- --unattendedmodeui none --mode unattended \
 && rm ./MPLABX-v${MPLABX_VERSION}-linux-installer.sh \
 \
 # Download and install PIC32 Legacy Peripheral Libraries (plib)
 && wget -O /tmp/plib.tar "http://ww1.microchip.com/downloads/en//softwarelibrary/pic32%20peripheral%20library/pic32%20legacy%20peripheral%20libraries%20linux%20(2).tar" \
 && tar -xvf /tmp/plib.tar -C /tmp \
 && rm /tmp/plib.tar \
 && mv /tmp/PIC32\ Legacy\ Peripheral\ Libraries.run /tmp/plib.run \
 && USER=root /tmp/plib.run --unattendedmodeui minimal --mode unattended \
    --prefix /opt/microchip/xc32/v${XC32_VERSION}/ \
 && rm /tmp/plib.run \
 \
 # Remove unneeded packages and apt-get info
 && apt-get purge --autoremove -qq \
    wget \
 && rm -rf /var/lib/apt/lists/* \
 \
 # Remove unneeded /opt/microchip/mplabx files
 && rm -rf \
    /opt/microchip/mplabx/v${MPLABX_VERSION}/packs/Atmel \
    /opt/microchip/mplabx/v${MPLABX_VERSION}/packs/arm \
 && find /opt/microchip/mplabx/v${MPLABX_VERSION}/packs/Microchip -mindepth 1 \
    -maxdepth 1 -not -name 'PIC32MX_DFP' -exec rm -rf {} \; \
 && find /opt/microchip/mplabx/v${MPLABX_VERSION}/packs/Microchip/PIC32MX_DFP/__version__/edc \
    -mindepth 1 -type f -not -name 'PIC32MX795F512L.PIC' -exec rm -rf {} \; \
 \
 # Remove unneeded /opt/microchip/xc32 files
 && rm -rf \
    /opt/microchip/xc32/v${XC32_VERSION}/docs \
    /opt/microchip/xc32/v${XC32_VERSION}/lib/gcc/pic32mx/*/fPIC \
    /opt/microchip/xc32/v${XC32_VERSION}/lib/gcc/pic32mx/*/fpu64 \
    /opt/microchip/xc32/v${XC32_VERSION}/lib/gcc/pic32mx/*/longdouble \
    /opt/microchip/xc32/v${XC32_VERSION}/lib/gcc/pic32mx/*/micromips \
    /opt/microchip/xc32/v${XC32_VERSION}/lib/gcc/pic32mx/*/mips16 \
    /opt/microchip/xc32/v${XC32_VERSION}/lib/gcc/pic32mx/*/no-float \
    /opt/microchip/xc32/v${XC32_VERSION}/lib/gcc/pic32mx/*/size \
    /opt/microchip/xc32/v${XC32_VERSION}/lib/gcc/pic32mx/*/speed \
    /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/fPIC \
    /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/fpu64 \
    /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/longdouble \
    /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/micromips \
    /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/mips16 \
    /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/no-float \
    /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/size \
    /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/speed \
 && find /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/include/proc -mindepth 1 \
    -maxdepth 1 -type f \( -not -name 'p32mx795f512l.h' -and -not -name \
    'p32mxgeneric.h' -and -not -name 'ppic32mx.h' \) -exec rm -rf {} \; \
 && find /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib -mindepth 1 \
    -maxdepth 1 -type f \( -name "libmchp_peripheral_*.a" -and -not -name \
    'libmchp_peripheral_32MX795F512L.a' \) -exec rm -rf {} \; \
 && find /opt/microchip/xc32/v${XC32_VERSION}/pic32mx/lib/proc -mindepth 1 \
    -maxdepth 1 -type d \( -not -name '32MX795F512L' -and -not -name \
    '32MXGENERIC' \) -exec rm -rf {} \;

ENV PATH=/opt/microchip/xc32/v${XC32_VERSION}/bin:$PATH
ENV PATH=/opt/microchip/mplabx/v${MPLABX_VERSION}/mplab_ide/bin:$PATH
