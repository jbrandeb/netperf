#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

FULL-PACKAGE-NAME=netperf
VERSION=2.8.0
BUG-REPORT-ADDRESS=netperf-feedback@netperf.org

AC_PREREQ([2.71])
AC_INIT([netperf],[2.8.0])

# Inherit compiler flags from the environment...
CFLAGS="${CFLAGS:=}"
CXXFLAGS="${CXXFLAGS:=}"

# use the target version rather than host - one day we may want cross-compile
AC_CANONICAL_TARGET
AC_CONFIG_SRCDIR([src/hist.h])
AM_INIT_AUTOMAKE([dist-zip])
AC_CONFIG_HEADERS([config.h])
AC_USE_SYSTEM_EXTENSIONS

AC_CONFIG_LIBOBJ_DIR(src/missing)

# make sure we build netperf_version.h
touch  ${ac_top_srcdir}src/netperf_version.h.in

# Checks for programs.
AC_PROG_CC
AC_PROG_RANLIB

AC_C_CONST

# Checks for libraries.
AC_CHECK_LIB([m],[main],[],[],[])ac_cv_lib_m=ac_cv_lib_m_main


# Checks for header files.
AC_CHECK_INCLUDES_DEFAULT
AC_PROG_EGREP

AC_HEADER_SYS_WAIT

# lets keep this in some semblence of alphabetical order
AC_CHECK_HEADERS([arpa/inet.h endian.h errno.h fcntl.h ifaddrs.h limits.h linux/tcp.h linux/socket.h malloc.h netdb.h netinet/in.h netinet/sctp.h sched.h signal.h stdlib.h string.h strings.h syscall.h sys/ioctl.h sys/mman.h sys/param.h sys/socket.h sys/stat.h sys/time.h sys/wait.h sys/ipc.h sys/sockio.h sys/sysinfo.h sys/wait.h stdlib.h unistd.h netinet/in_systm.h netinet/ip.h sys/sendfile.h])

# Some platforms require these.  There may be a better way.
AC_CHECK_LIB([socket],[main],[],[],[])ac_cv_lib_socket=ac_cv_lib_socket_main

if test "$ac_cv_lib_socket_main" = yes ; then
    AC_CHECK_LIB([nsl],[main],[],[],[])ac_cv_lib_nsl=ac_cv_lib_nsl_main

    AC_CHECK_LIB([sendfile],[main],[],[],[])ac_cv_lib_sendfile=ac_cv_lib_sendfile_main

    AC_SYS_LARGEFILE
fi

# this one is for Tru64 and bind_to_cpu_id
AC_CHECK_LIB([mach],[main],[],[],[])ac_cv_lib_mach=ac_cv_lib_mach_main


# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_TYPE_OFF_T
AC_TYPE_SIZE_T
# AC_TYPE_SOCKLEN_T

OLD_TYPE_SOCKLEN_T

# AC_TYPE_IN_PORT_T
AC_DECL_H_ERRNO
AC_STRUCT_SOCKADDR_STORAGE
AC_CHECK_HEADERS_ONCE([sys/time.h])

AC_HEADER_STDBOOL
AC_CHECK_SA_LEN(ac_cv_sockaddr_has_sa_len)

# Checks for library functions.
# AC_FUNC_ERROR_AT_LINE
AC_FUNC_FORK
# AC_FUNC_MALLOC
AC_FUNC_MMAP
AC_FUNC_SELECT_ARGTYPES
AC_FUNC_SETPGRP
# AC_FUNC_STAT
# remove pstat_getdynamic (at least for now) since we don't do
# anything conditional with the check anyway...
AC_CHECK_FUNCS([alarm bzero daemon gethostbyname gethrtime gettimeofday inet_ntoa memset memcpy munmap select setsid socket sqrt strcasecmp strchr strstr strtoul uname toupper])

#AC_CONFIG_SUBDIRS(src/missing)

# does this platform need the replacement getaddrinfo
AC_CHECK_FUNCS([getnameinfo getaddrinfo inet_ntop getifaddrs])
# AC_REPLACE_FUNCS([getaddrinfo])


if test "$ac_cv_func_getaddrinfo$ac_cv_func_getnameinfo" != yesyes ; then
   AC_MSG_NOTICE([Requesting replacement getaddrinfo/getnameinfo])
   AC_LIBOBJ(getaddrinfo)
   HAVE_MISSING=yes
fi
if test "$ac_cv_func_inet_ntop" != yes ; then
   AC_MSG_NOTICE([Requesting replacement inet_ntop])
   AC_LIBOBJ(inet_ntop)
   HAVE_MISSING=yes
fi
AM_CONDITIONAL(NEED_LIBCOMPAT, test "$HAVE_MISSING" = "yes")

# check for POSIX asynchronous I/O routines
AC_SEARCH_LIBS(aio_read, rt,
	[AC_DEFINE(HAVE_AIO, 1,
    	[Define to 1 if your system has POSIX asynchronous I/O functions. ])])

AC_CHECK_FUNCS(sendfile)

AC_CHECK_FUNCS(uname)

# check for the various CPU binding calls
AC_CHECK_FUNCS(mpctl processor_bind sched_setaffinity bind_to_cpu_id bindprocessor)

# see if we should be enabling histogram support

AC_MSG_CHECKING(whether to include histogram support)

AC_ARG_ENABLE(histogram,
	[AS_HELP_STRING([--enable-histogram],[include individual op timing, may affect result]) ])

case "$enable_histogram" in
	yes)
		use_histogram=true
		;;
	no)
		use_histogram=false
		;;
	'')
		# whatever
		use_histogram=false
		;;
	*)
		AC_MSG_ERROR([--enable-histogram takes yes or no])
		;;
esac

if $use_histogram
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_histogram
then
	AC_DEFINE([WANT_HISTOGRAM],,[Define to one to enable histogram support. May affect results.])
fi
# see if we should be enabling histogram support

AC_MSG_CHECKING(whether to include dirty support)

AC_ARG_ENABLE(dirty,
	[AS_HELP_STRING([--enable-dirty],[write to buffers each time, may affect result]) ])

case "$enable_dirty" in
	yes)
		use_dirty=true
		;;
	no)
		use_dirty=false
		;;
	'')
		# whatever
		use_dirty=false
		;;
	*)
		AC_MSG_ERROR([--enable-dirty takes yes or no])
		;;
esac

if $use_dirty
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_dirty
then
	AC_DEFINE([DIRTY],,[Define to one to enable dirty buffer support. May affect results.])
fi

# see if we should be enabling demo support

AC_MSG_CHECKING(whether to include demo support)

AC_ARG_ENABLE(demo,
	[AS_HELP_STRING([--enable-demo],[emit interim results during the run. May affect results.])])

case "$enable_demo" in
	yes)
		use_demo=true
		;;
	no)
		use_demo=false
		;;
	'')
		# whatever
		use_demo=false
		;;
	*)
		AC_MSG_ERROR([--enable-demo takes yes or no])
		;;
esac

if $use_demo
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_demo
then
	AC_DEFINE([WANT_DEMO],,[Define to one to enable demo support. May affect results.])
fi

# see if we should be including the AF_UNIX tests

AC_MSG_CHECKING(whether to include Unix-domain socket tests)

AC_ARG_ENABLE(unixdomain,
	[AS_HELP_STRING([--enable-unixdomain],[include Unix Domain socket tests])])

case "$enable_unixdomain" in
     yes)
		use_unixdomain=true
		;;
     no)
		use_unixdomain=false
		;;
     '')
		use_unixdomain=false
		;;
     *)
		AC_MSG_ERROR([--enable-unixdomain takes yes or no])
		;;
esac

if $use_unixdomain
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_unixdomain
then
	AC_DEFINE([WANT_UNIX],,[Define to one to include Unix Domain socket tests.])
fi

# see if we should be including the DLPI tests

AC_MSG_CHECKING(whether to include DLPI tests)

AC_ARG_ENABLE(dlpi,
	[AS_HELP_STRING([--enable-dlpi],[include DLPI (link-layer) tests])])

case "$enable_dlpi" in
     yes)
		use_dlpi=true
		;;
     no)
		use_dlpi=false
		;;
     '')
		use_dlpi=false
		;;
     *)
		AC_MSG_ERROR([--enable-dlpi takes yes or no])
		;;
esac

if $use_dlpi
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_dlpi
then
	AC_DEFINE([WANT_DLPI],,[Define to one to include DLPI tests.])
fi

# see if we should be including the DCCP tests

AC_MSG_CHECKING(whether to include DCCP tests)

AC_ARG_ENABLE(dccp,
	[AS_HELP_STRING([--enable-dccp],[include DCCP tests])])

case "$enable_dccp" in
     yes)
		use_dccp=true
		;;
     no)
		use_dccp=false
		;;
     '')
		use_dccp=false
		;;
     *)
		AC_MSG_ERROR([--enable-dccp takes yes or no])
		;;
esac

if $use_dccp
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_dccp
then
	AC_DEFINE([WANT_DCCP],,[Define to one to include DCCP tests.])
fi

# see if we should be including the OMNI tests

AC_MSG_CHECKING(whether to include OMNI tests)

AC_ARG_ENABLE(omni,
	[AS_HELP_STRING([--enable-omni],[include OMNI tests])])

case "$enable_omni" in
     yes)
		use_omni=true
		;;
     no)
		use_omni=false
		;;
     '')
		use_omni=true
		;;
     *)
		AC_MSG_ERROR([--enable-omni takes yes or no])
		;;
esac

if $use_omni
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_omni
then
	AC_DEFINE([WANT_OMNI],,[Define to one to include OMNI tests.])
	AC_DEFINE([WANT_MIGRATION],,[Define to one to migrate classic to OMNI tests.])
fi


# see if we should be including the XTI tests

AC_MSG_CHECKING(whether to include XTI tests)

AC_ARG_ENABLE(xti,
	[AS_HELP_STRING([--enable-xti],[include XTI socket tests])])

case "$enable_xti" in
     yes)
		use_xti=true
		;;
     no)
		use_xti=false
		;;
     '')
		use_xti=false
		;;
     *)
		AC_MSG_ERROR([--enable-xti takes yes or no])
		;;
esac

if $use_xti
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_xti
then
	AC_DEFINE([WANT_XTI],,[Define to one to include XTI tests.])
fi

# see if we should be including the SDP tests

AC_MSG_CHECKING(whether to include SDP tests)

AC_ARG_ENABLE(sdp,
	[AS_HELP_STRING([--enable-sdp],[include SDP socket tests])])

case "$enable_sdp" in
     yes)
		# probably need to be a bit more sophisticated here
		AC_CHECK_LIB(sdp,t_open)
		use_sdp=true
		;;
     no)
		use_sdp=false
		;;
     '')
		use_sdp=false
		;;
     *)
		AC_MSG_ERROR([--enable-sdp takes yes or no])
		;;
esac

if $use_sdp
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_sdp
then
	AC_DEFINE([WANT_SDP],,[Define to one to include SDP tests.])
fi

# see if we should be including the ICSC-EXS tests

AC_MSG_CHECKING(whether to include ICSC-EXS tests)

AC_ARG_ENABLE(exs,
	[AS_HELP_STRING([--enable-exs],[include ICSC async sockets tests])])

case "$enable_exs" in
     yes)
		use_exs=true
		AC_CHECK_HEADER(sys/exs.h,,[use_exs=false])
		AC_CHECK_LIB(exs,exs_init,,[use_exs=false])
		;;
     no)
		use_exs=false
		;;
     '')
		use_exs=false
		;;
     *)
		AC_MSG_ERROR([--enable-exs takes yes or no])
		;;
esac

if $use_exs
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_exs
then
	AC_DEFINE([HAVE_ICSC_EXS],,[Define to one to include ICSC-EXS tests.])
fi

# see if we should be enabling SCTP support


AC_ARG_ENABLE(sctp,
	[AS_HELP_STRING([--enable-sctp],[include tests to measure SCTP performance ])])

case "$enable_sctp" in
	yes)
		use_sctp=true
		AC_CHECK_HEADERS(netinet/sctp.h,,use_sctp=false,
[[
#include <sys/socket.h>
]])
		case "$host" in
		*-*-freebsd[[7-9]].* | *-*-freebsd1[[0-2]].* )
			# FreeBSD 7.x and later SCTP support doesn't need -lsctp.
			;;
		*)
			AC_CHECK_LIB([sctp],[main],[],[use_sctp=false],[])ac_cv_lib_sctp=ac_cv_lib_sctp_main

			;;
		esac
		AC_CHECK_MEMBER(struct sctp_event_subscribe.sctp_adaptation_layer_event,
				, , [#include <netinet/sctp.h>])
		if test "$ac_cv_member_struct_sctp_event_subscribe_sctp_adaptation_layer_event" = "yes"; then
			AC_DEFINE([HAVE_SCTP_ADAPTATION_LAYER_EVENT], 1,
				  [Define to 1 if `struct sctp_event_subscribe' has a `sctp_adaptation_layer_event' member])
		fi
		;;
	no)
		use_sctp=false
		;;
	'')
		# whatever
		use_sctp=false
		;;
	*)
		AC_MSG_ERROR([--enable-sctp takes yes or no])
		;;
esac

AC_MSG_CHECKING(whether to include SCTP tests)

if $use_sctp
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_sctp
then
	AC_DEFINE([WANT_SCTP],,[Define to one to include SCTP tests.])
fi

# see if we should be enabling paced sends

AC_MSG_CHECKING([whether to include paced send (intervals) support])

AC_ARG_ENABLE(intervals,
	[AS_HELP_STRING([--enable-intervals],[include ability to pace operations, may affect result])])

case "$enable_intervals" in
	yes)
		use_intervals=true
		;;
	no)
		use_intervals=false
		;;
	'')
		use_intervals=false
		;;
	*)
		AC_MSG_ERROR([--enable-intervals takes yes or no])
		;;
esac

if $use_intervals
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_intervals
then
	AC_DEFINE([WANT_INTERVALS],,[Define to one to enable paced operation support. May affect results.])
fi

# see if paced sends should wait and spin

AC_MSG_CHECKING([whether paced sends should spin])

AC_ARG_ENABLE(spin,
	[AS_HELP_STRING([--enable-spin],[paced operations (--enable-intervals) should sit and spin - WILL affect result])])

case "$enable_spin" in
	yes)
		use_spin=true
		;;
	no)
		use_spin=false
		;;
	'')
		use_spin=false
		;;
	*)
		AC_MSG_ERROR([--enable-spin takes yes or no])
		;;
esac

if $use_spin
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_spin
then
	AC_DEFINE([WANT_INTERVALS],,[Define to one to enable paced operation support. May affect results.])
	AC_DEFINE([WANT_SPIN],,[Define to one to spin waiting on paced operation. WILL AFFEFCT CPU UTILIZATION])
fi

# see if we should be enabling initial request bursts

AC_MSG_CHECKING([whether to include initial burst support in _RR tests])

AC_ARG_ENABLE(burst,
	[AS_HELP_STRING([--enable-burst],[include intial request burst ability in _RR tests, may affect result])])

case "$enable_burst" in
	yes)
		use_burst=true
		;;
	no)
		use_burst=false
		;;
	'')
		use_burst=true
		;;
	*)
		AC_MSG_ERROR([--enable-burst takes yes or no])
		;;
esac

if $use_burst
then
	AC_MSG_RESULT(yes)
else
	AC_MSG_RESULT(no)
fi

if $use_burst
then
	AC_DEFINE([WANT_FIRST_BURST],,[Define to one to enable initial _RR burst support. May affect results.])
fi

# time to see about CPU utilization measurements

AC_MSG_CHECKING([which CPU utilization measurement type to use])

AC_ARG_ENABLE(cpuutil,
	[AS_HELP_STRING([--enable-cpuutil],[include code to measure CPU utilization using specified mechanism])])

NETCPU_SOURCE="$enable_cpuutil"
case "$enable_cpuutil" in
	pstat)
		use_cpuutil=true
		AC_DEFINE([USE_PSTAT],,[Use HP-UX's pstat interface to measure CPU util.])
		;;
	pstatnew)
		use_cpuutil=true
		AC_DEFINE([USE_PSTAT],,[Use HP-UX's pstat interface to measure CPU util.])
		;;
        perfstat)
                use_cpuutil=true
                AC_DEFINE([USE_PERFSTAT],,[Use AIX's perfstat interface to measure CPU util.])
		AC_CHECK_LIB([perfstat],[main],[],[],[])ac_cv_lib_perfstat=ac_cv_lib_perfstat_main

                ;;

	looper)
		use_cpuutil=true
		AC_DEFINE([USE_LOOPER],,[Use looper/soaker processes to measure CPU util.])
		;;
	procstat)
		use_cpuutil=true
		AC_DEFINE([USE_PROC_STAT],,[Use Linux's procstat interface to measure CPU util.])
		;;
	kstat)
		use_cpuutil=true
		AC_DEFINE([USE_KSTAT],,[Use Solaris's kstat interface to measure CPU util.])
		AC_CHECK_LIB([kstat],[main],[],[],[])ac_cv_lib_kstat=ac_cv_lib_kstat_main

		;;
	kstat10)
		use_cpuutil=true
		AC_DEFINE([USE_KSTAT],,[Use Solaris's kstat interface to measure CPU util.])
		AC_CHECK_LIB([kstat],[main],[],[],[])ac_cv_lib_kstat=ac_cv_lib_kstat_main

		;;
	osx)
		use_cpuutil=true
		AC_DEFINE([USE_OSX],,[Use MacOS X's host_info interface to measure CPU util.])
		;;
	'')
# ia64-hp-hpux11.23
# i386-pc-solaris2.10
# guess it automagically in a nice big case statement
		case $target in
		     *-*-linux*)
			use_cpuutil=true
			AC_DEFINE([USE_PROC_STAT],,[Use Linux's procstat interface to measure CPU util.])
			enable_cpuutil="procstat - auto"
			NETCPU_SOURCE="procstat"
			;;
		     *-*-hpux11.23 | *-*-hpux11.31)
			use_cpuutil=true
			AC_DEFINE([USE_PSTAT],,[Use HP-UX's pstat interface to measure CPU util.])
			enable_cpuutil="pstatnew - auto"
			NETCPU_SOURCE="pstatnew"
			;;
		     *-*-hpux11* | *-*-hpux10*)
			use_cpuutil=true
			AC_DEFINE([USE_PSTAT],,[Use HP-UX's pstat interface to measure CPU util.])
			enable_cpuutil="pstat - auto"
			NETCPU_SOURCE="pstat"
			;;
		     *-*-aix5.* | *-*-aix6.*)
			use_puutil=true
			AC_DEFINE([USE_PERFSTAT],,[Use AIX's perfstat interface to measure CPU util.])
			AC_CHECK_LIB([perfstat],[main],[],[],[])ac_cv_lib_perfstat=ac_cv_lib_perfstat_main

			enable_cpuutil="perfstat - auto"
			NETCPU_SOURCE="perfstat"
			;;
		     *-*-solaris2.1*)
			use_cpuutil=true
			AC_DEFINE([USE_KSTAT],,[Use Solaris's kstat interface to measure CPU util.])
			AC_CHECK_LIB([kstat],[main],[],[],[])ac_cv_lib_kstat=ac_cv_lib_kstat_main

			enable_cpuutil="kstat10 - auto"
			NETCPU_SOURCE="kstat10"
			;;
		     *-*-solaris2.*)
			use_cpuutil=true
			AC_DEFINE([USE_KSTAT],,[Use Solaris's kstat interface to measure CPU util.])
			AC_CHECK_LIB([kstat],[main],[],[],[])ac_cv_lib_kstat=ac_cv_lib_kstat_main

			enable_cpuutil="kstat - auto"
			NETCPU_SOURCE="kstat"
			;;
                     *-*-freebsd[[4-9]].* | *-*-freebsd1[[0-2]].* | *-*-netbsd[[1-9]].* )
			use_cpuutil=true
			AC_DEFINE([USE_SYSCTL],,[Use MumbleBSD's sysctl interface to measure CPU util.])
			enable_cpuutil="sysctl - auto"
			NETCPU_SOURCE="sysctl"
			;;
		    *-*-darwin*)
			use_cpuutil=true
			AC_DEFINE([USE_OSX],,[Use MacOS X's host_info interface to measure CPU util.])
			enable_cpuutil="osx - auto"
			NETCPU_SOURCE="osx"
			;;
		     *)
			use_cpuutil=false
			NETCPU_SOURCE="none"
			enable_cpuutil="none. Consider teaching configure about your platform."
			;;
		esac
		;;
	none)
		use_cpuutil=false
		;;
	*)
		AC_MSG_ERROR([--enable-cpuutil takes kstat, kstat10, looper, osx, perfstat, procstat, pstat, pstatnew, sysctl or none])
		;;
esac

	AC_MSG_RESULT("$enable_cpuutil")

AC_SUBST(NETCPU_SOURCE)


# now spit it all out
AC_CONFIG_FILES([Makefile
                 src/netperf_version.h
                 src/Makefile
		 src/missing/Makefile
		 src/missing/m4/Makefile
		 doc/Makefile
		 doc/examples/Makefile
                 netperf.spec])

AC_OUTPUT
