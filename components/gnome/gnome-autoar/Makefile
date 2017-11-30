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
# Copyright (c) 2017, Oracle and/or its affiliates. All rights reserved.
#

BUILD_BITS= 		64_and_32
COMPILER=		gcc
include ../../../make-rules/shared-macros.mk

COMPONENT_NAME=		gnome-autoar
COMPONENT_VERSION=	0.2.2
COMPONENT_PROJECT_URL=	https://developer.gnome.org/gnome-autoar/ 
COMPONENT_ARCHIVE_HASH= \
    sha256:e1fe2c06eed30305c38bf0939c72b0e51b4716658e2663a0cf4a4bf57874ca62
COMPONENT_BUGDB=        gnome/libraries
COMPONENT_ANITYA_ID=	15236

TPNO=			34570

# Test directory exists, but gmake test doesn't provide anything useful
TEST_TARGET= $(SKIP_TEST)

# Needs gobject-introspection, not available in S11
include $(WS_MAKE_RULES)/no-evaluation.mk

include $(WS_MAKE_RULES)/gnome-component.mk

CONFIGURE_OPTIONS += --disable-static
CONFIGURE_OPTIONS += --enable-gtk-doc

REQUIRED_PACKAGES += library/desktop/gtk3
REQUIRED_PACKAGES += library/glib2
REQUIRED_PACKAGES += library/libarchive