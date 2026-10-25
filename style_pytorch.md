Extends `~/.agents/style.md` for PyTorch model code (any architecture).

- Write `torch.nn.Linear`, `torch.nn.functional.relu` in full; no `nn` or `F` aliases.
- Name a submodule by its standard abbreviation or role as the root, qualified by what it acts on: `encoder`, `decoder`, `proj_input`/`proj_output`, `ln_mhsa` (LayerNorm after MHSA), `proj_codebook`. Stacks end in `_stack` (`encoder_stack`).
- Keep the paper's symbols where code computes its formula (`Q`, `K`, `W_Q`, `Z_1`, `z_e`, `mu`), the only exception to lowercase names. Follow the paper's order, one named step per line.
- `forward` takes `input`; other streams are named by role (`ca_input`, `z`). Sublayer results are `<sublayer>_output`, rebound through dropout or activation. Masks put the kind first (`mask_causal`, `mask_padding`). Set a flag (`is_ca`) before branching on an optional input.
- Start `forward` by unpacking the shape into named lengths and dimensions (`len_batch, len_input, d_input = input.size()`, `_` for unused), then `assert` dimensions with an f-string showing both values. `assert` for tensor shapes, `ValueError` for config.
- Comments may also cite the formula a block implements (`# QK^T/sqrt(d_sa)`, `# z = mu + sigma * eps`) or the shape after a reshape or permute (`# To (len_batch, n_head, len_input, d_sa)`).
- Class docstrings: one-line title, formula or layer order, and `Args:` with the paper's default value (`d_model: 512 model hidden dimension`).
