#
# CDDL HEADER START
#
# The contents of this file are subject to the terms of the
# Common Development and Distribution License (the "License").
# You may not use this file except in compliance with the License.
#
# You can obtain a copy of the license at usr/src/OPENSOLARIS.LICENSE
# or http://www.opensolaris.org/os/licensing.
# See the License for the specific language governing permissions
# and limitations under the License.
#
# When distributing Covered Code, include this CDDL HEADER in each
# file and include the License file at usr/src/OPENSOLARIS.LICENSE.
# If applicable, add the following below this CDDL HEADER, with the
# fields enclosed by brackets "[]" replaced with your own identifying
# information: Portions Copyright [yyyy] [name of copyright owner]
#
# CDDL HEADER END
#

#
# Copyright (c) 2016, 2020, Oracle and/or its affiliates.
#

BUILD_BITS =            64_and_32
# All components using gobject-introspection must be built with gcc as
# gobject-introspection does not produce correct results when using Studio.
COMPILER =              gcc
include ../../../make-rules/shared-macros.mk

COMPONENT_NAME=		accountsservice
COMPONENT_VERSION=	0.6.43
COMPONENT_PROJECT_URL=	http://freedesktop.org/wiki/Software/AccountsService/
COMPONENT_SRC=		$(COMPONENT_NAME)-$(COMPONENT_VERSION)
COMPONENT_ARCHIVE=	$(COMPONENT_SRC).tar.xz
COMPONENT_ARCHIVE_HASH= \
	sha256:ed3ba94aa38ceb822a0e1a1ac71bf1a8123babf90be049397b3a00900e48d6cc
COMPONENT_ARCHIVE_URL=	http://freedesktop.org/software/accountsservice/$(COMPONENT_ARCHIVE)
COMPONENT_BUGDB=	gnome/libraries
COMPONENT_ANITYA_ID=	13107

TPNO=			34437

TEST_TARGET = $(NO_TESTS)

SMF_MANIFESTS= $(COMPONENT_DIR)/files/accountsservice.xml

include $(WS_MAKE_RULES)/common.mk

# Assumes GNU xgettext in PATH
COMPONENT_BUILD_ENV += PATH=$(GNUBIN):$(PATH)
CONFIGURE_ENV += PATH=$(GNUBIN):$(PATH)

# GNU Build System files have to be regenerated for automake 1.16.
COMPONENT_PREP_ACTION += \
    ( cd $(@D) ; autoreconf -fiv );

# Solaris doesn't have a defined authpriv facility in syslog, so map it to
# an unused one for now
CPPFLAGS += -DLOG_AUTHPRIV='(10<<3)'

CONFIGURE_OPTIONS += --enable-docbook-docs 
CONFIGURE_OPTIONS += --enable-gtk-doc
CONFIGURE_OPTIONS += --enable-shared
CONFIGURE_OPTIONS += --disable-static
CONFIGURE_OPTIONS += --disable-systemd
CONFIGURE_OPTIONS += --with-pic
CONFIGURE_OPTIONS += --libexecdir="$(USRLIBDIR)"
CONFIGURE_OPTIONS += --localstatedir="$(VARDIR)"
CONFIGURE_OPTIONS += --sysconfdir="$(ETCDIR)"

# Solaris reserves uids < 100 for system accounts
CONFIGURE_OPTIONS += --with-minimum-uid=100

# Solaris does not have a wheel group, so we use the "sysadmin" group to
# determine which accounts should be ACT_USER_ACCOUNT_TYPE_ADMINISTRATOR
CONFIGURE_OPTIONS += --enable-admin-group=sysadmin

# Only need to install library from 32-bit builds, everything else overlaps
# with 64-bit builds
COMPONENT_INSTALL_ARGS.32 += SUBDIRS=src/libaccountsservice
COMPONENT_INSTALL_ARGS += $(COMPONENT_INSTALL_ARGS.$(BITS))

# SMF manifest & method
COMPONENT_POST_INSTALL_ACTION.64 += \
        $(INSTALL) -D -m 0444 $(COMPONENT_DIR)/files/accountsservice.xml \
                $(PROTO_DIR)/lib/svc/manifest/system/accountsservice.xml; \
        $(INSTALL) -D -m 0555 $(COMPONENT_DIR)/files/svc-accountsservice \
                $(PROTO_DIR)/lib/svc/method/svc-accountsservice; 

REQUIRED_PACKAGES += developer/documentation-tool/xmlto
REQUIRED_PACKAGES += library/desktop/gobject/gobject-introspection
REQUIRED_PACKAGES += library/glib2
REQUIRED_PACKAGES += library/libxslt
REQUIRED_PACKAGES += system/core-os
REQUIRED_PACKAGES += system/library/dbus
REQUIRED_PACKAGES += system/library/polkit
