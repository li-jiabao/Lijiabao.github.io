
# Solaris and Linux Support

## Solaris 10

To test that SDP is enabled use the `sdpadm`(1M) command:

```

% /usr/sbin/sdpadm status
SDP is Enabled

```

You can use the grep command to search the `/etc/path_to_inst` file for the string "ibd" to view a list of IB adaptors that are supported on your network.

Other commands you might find useful are `ib`(7D), `ibd`(7D), and `sdp`(7D).

## Solaris 11

You can use the following command to obtain the InfiniBand partition link information:

```

% dladm show-port -o LINK

```

Other commands you might find useful are `ib`(7D), `ibd`(7D), and `sdp`(7D).

## Linux

You can use the following command to obtain the list of InfiniBand adapters:

```

% egrep "^[ \t]+ib" /proc/net/dev

```
