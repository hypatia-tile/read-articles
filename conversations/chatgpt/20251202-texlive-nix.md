You can add TeX Live to your Nix-Darwin setup by just extending the packages you already install via Nix (ideally through Home Manager in `nix/home`, like you’ve been doing for other CLI tools).

Below is a concrete way to do it that should fit nicely into your repo.

---

## 1. Decide how “big” TeX Live should be

Nixpkgs exposes whole TeX Live “schemes” as single packages: ([NixOS Wiki][1])

Typical choices:

* **`texlive.combined.scheme-small`** – reasonable, includes LaTeX, xetex, some extras
* **`texlive.combined.scheme-medium`** – more languages/packages
* **`texlive.combined.scheme-full`** – *everything* (very big, but you never hit “missing package”)

I’ll use `scheme-medium` in the example; you can swap it for `scheme-full` if you want the “install everything” version.

---

## 2. Add a Home Manager module for TeX Live

Create a new file, for example:

`nix/home/tex.nix`:

```nix
{ pkgs, ... }:

{
  # Install TeX Live into your user environment
  home.packages = with pkgs; [
    texlive.combined.scheme-medium
    # or: texlive.combined.scheme-full
  ];
}
```

If you prefer to install it system-wide instead of per-user, you can do the same thing in your Darwin module:

```nix
{ pkgs, ... }:

{
  environment.systemPackages = with pkgs; [
    texlive.combined.scheme-medium
  ];
}
```

…but given how you’ve been using Home Manager, I’d keep it under `home.packages`.

---

## 3. Import this module into your existing Home config

In your main Home Manager module (something like `nix/home/default.nix` where you already have `imports = [ ... ];`), add `./tex.nix`:

```nix
{ pkgs, ... }:

{
  imports = [
    ./zsh.nix
    ./neovim.nix
    ./tex.nix
  ];

  # ... your existing home.* options ...
}
```

(Adjust paths/module names to match your actual structure, but the idea is just “add tex.nix to imports”.)

---

## 4. Rebuild with nix-darwin

From your `nix-darwin` repo:

```zsh
cd ~/github/nix-darwin
sudo darwin-rebuild switch --flake .#"Kazukis-MacBook-Air"
```

After the build finishes, confirm TeX Live is on your PATH:

```zsh
pdflatex --version
lualatex --version
```

You should see the TeX Live version from Nix.

---

If you tell me whether you’d rather:

* keep TeX *only* in project devShells, or
* have a global but “minimal” LaTeX set, or
* go all-in with `scheme-full`,

I can tweak the exact `texlive` expression (e.g. using `texlive.withPackages` for a custom mix tailored to LuaLaTeX + your usual packages).

[1]: https://nixos.wiki/wiki/TexLive?utm_source=chatgpt.com "TexLive - NixOS Wiki"
