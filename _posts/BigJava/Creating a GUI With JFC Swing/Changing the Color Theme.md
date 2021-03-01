
# Changing the Color Theme

The Nimbus look and feel has a set of default colors, but you are not required to use them. You can change the colors to match your corporate brand or other color scheme.

All of the colors used by Nimbus are stored as a set of `UIManager` properties. You can change any or all of these properties before you set the look and feel. For example:

```

<b>UIManager.put("nimbusBase", new Color(...));
UIManager.put("nimbusBlueGrey", new Color(...));
UIManager.put("control", new Color(...));</b>

for (LookAndFeelInfo info : UIManager.getInstalledLookAndFeels()) {
    if ("Nimbus".equals(info.getName())) {
        UIManager.setLookAndFeel(info.getClassName());
        break;
    }
}

```

These three base colors, `nimbusBase`, `nimbusBlueGrey`, and `control`, will address most of your needs. See a full list of color keys and their default values on the 
[Nimbus Defaults](_nimbusDefaults.html#primary) page.
