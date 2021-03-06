
<a name="0x1_MigrateWallets"></a>

# Module `0x1::MigrateWallets`


<a name="@Summary_0"></a>

## Summary

Module providing method to convert all wallets to "slow wallets"
migrations should have own module, since imports can cause dependency cycling.


-  [Summary](#@Summary_0)
-  [Constants](#@Constants_1)
-  [Function `migrate_slow_wallets`](#0x1_MigrateWallets_migrate_slow_wallets)


<pre><code><b>use</b> <a href="CoreAddresses.md#0x1_CoreAddresses">0x1::CoreAddresses</a>;
<b>use</b> <a href="DiemAccount.md#0x1_DiemAccount">0x1::DiemAccount</a>;
<b>use</b> <a href="Migrations.md#0x1_Migrations">0x1::Migrations</a>;
<b>use</b> <a href="ValidatorUniverse.md#0x1_ValidatorUniverse">0x1::ValidatorUniverse</a>;
<b>use</b> <a href="../../../../../../move-stdlib/docs/Vector.md#0x1_Vector">0x1::Vector</a>;
</code></pre>



<a name="@Constants_1"></a>

## Constants


<a name="0x1_MigrateWallets_UID"></a>



<pre><code><b>const</b> <a href="Migrations.md#0x1_MigrateWallets_UID">UID</a>: u64 = 10;
</code></pre>



<a name="0x1_MigrateWallets_migrate_slow_wallets"></a>

## Function `migrate_slow_wallets`



<pre><code><b>public</b> <b>fun</b> <a href="Migrations.md#0x1_MigrateWallets_migrate_slow_wallets">migrate_slow_wallets</a>(vm: &signer)
</code></pre>



<details>
<summary>Implementation</summary>


<pre><code><b>public</b> <b>fun</b> <a href="Migrations.md#0x1_MigrateWallets_migrate_slow_wallets">migrate_slow_wallets</a>(vm: &signer) {
  <a href="CoreAddresses.md#0x1_CoreAddresses_assert_diem_root">CoreAddresses::assert_diem_root</a>(vm);
  <b>if</b> (!<a href="Migrations.md#0x1_Migrations_has_run">Migrations::has_run</a>(<a href="Migrations.md#0x1_MigrateWallets_UID">UID</a>)) {
    <b>let</b> vec_addr = <a href="ValidatorUniverse.md#0x1_ValidatorUniverse_get_eligible_validators">ValidatorUniverse::get_eligible_validators</a>(vm);
    // TODO: how <b>to</b> get other accounts?

    // tag all accounts <b>as</b> slow wallets
    <b>let</b> len = <a href="../../../../../../move-stdlib/docs/Vector.md#0x1_Vector_length">Vector::length</a>&lt;address&gt;(&vec_addr);
    <b>let</b> i = 0;
    <b>while</b> (i &lt; len) {
      <b>let</b> addr = *<a href="../../../../../../move-stdlib/docs/Vector.md#0x1_Vector_borrow">Vector::borrow</a>&lt;address&gt;(&vec_addr, i);
      <a href="DiemAccount.md#0x1_DiemAccount_vm_migrate_slow_wallet">DiemAccount::vm_migrate_slow_wallet</a>(vm, addr);
      i = i + 1;
    };
    <a href="Migrations.md#0x1_Migrations_push">Migrations::push</a>(vm, <a href="Migrations.md#0x1_MigrateWallets_UID">UID</a>, b"<a href="Migrations.md#0x1_MigrateWallets">MigrateWallets</a>");
  };
}
</code></pre>



</details>


[//]: # ("File containing references which can be used from documentation")
[ACCESS_CONTROL]: https://github.com/diem/dip/blob/main/dips/dip-2.md
[ROLE]: https://github.com/diem/dip/blob/main/dips/dip-2.md#roles
[PERMISSION]: https://github.com/diem/dip/blob/main/dips/dip-2.md#permissions
