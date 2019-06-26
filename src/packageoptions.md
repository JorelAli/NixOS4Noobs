# Setting package options




nixpkgs.config = {
    allowUnfree = true;

    adb.enable = true;

    chromium = {
      jre = false;
      enableGoogleTalkPlugin = true;
      enablePepperPDF = true;
    };

    packageOverrides = pkgs: rec {
      neovim = (import ./vim.nix);
    };
  };
